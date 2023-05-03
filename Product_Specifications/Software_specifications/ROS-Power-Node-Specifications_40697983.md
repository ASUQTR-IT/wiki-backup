1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Product Specifications](Product-Specifications_37355662.html)
4. [Software Specifications](Software-Specifications_42827905.html)

# Sous-Marin ASUQTR : ROS Power Node Specifications

Created by Unknown User (laurieb), last modified by Gabriel Tremblay on Nov 20, 2020

||
<colgroup><col /><col /></colgroup>|---|
|Target release|1.0|
|Task|[![](https://jira.asuqtr.com/secure/viewavatar?size=xsmall&avatarId=10318&avatarType=issuetype)ASUQTR-618](https://jira.asuqtr.com/browse/ASUQTR-618?src=confmacro) - Faire la documentation pour la power\_node Done|
|Document status|DRAFT|
|Document owner|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|Designer|[Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg) [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow)|
|Developers|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|QA||

* [General Abstract](#ROSPowerNodeSpecifications-GeneralAbstract)

  * [Power\_node's Main Goal](#ROSPowerNodeSpecifications-Power_node'sMainGoal)
  * [Serial Communication](#ROSPowerNodeSpecifications-SerialCommunication)
* [USB Ports Protocol](#ROSPowerNodeSpecifications-USBPortsProtocol)

  * [What Helper & Driver Stand For?](#ROSPowerNodeSpecifications-WhatHelper&DriverStandFor?)
  * [Helper & Driver USB Ports](#ROSPowerNodeSpecifications-Helper&DriverUSBPorts)
  * [Protocol & Thread Classes](#ROSPowerNodeSpecifications-Protocol&ThreadClasses)
* [JSON to ROS msgs](#ROSPowerNodeSpecifications-JSONtoROSmsgs)

  * [Pod\_Ros\_Msgs dictionary](#ROSPowerNodeSpecifications-Pod_Ros_Msgsdictionary)
  * [From Dictionary To Ros Msgs](#ROSPowerNodeSpecifications-FromDictionaryToRosMsgs)
* [Power\_Node Dictionaries](#ROSPowerNodeSpecifications-Power_NodeDictionaries)

  * [Pod\_Ros\_Msgs Dictionary's Key List](#ROSPowerNodeSpecifications-Pod_Ros_MsgsDictionary'sKeyList)
  * [Opened\_Ports Dictionary's Key List](#ROSPowerNodeSpecifications-Opened_PortsDictionary'sKeyList)
  * [Handlers Dictionary's Key List](#ROSPowerNodeSpecifications-HandlersDictionary'sKeyList)

# **General Abstract**

## *Power\_node's Main Goal*

* Read serialized data coming from Arduino board in each PCB Pod\*
* Monitoring battery cells, water leak & temperature anomalies from PCB Pods:
  * Measure battery pack cells's voltage
  * Detect high/low state from leak sensor
  * Measure thermistor's voltage
  * Indicate battery pack's charge & PCB pods's anomalies with adressable LEDs

*\*For more informations about PCB pods & its different components see:* [PCB Pod Specifications](PCB-Pod-Specifications_37355667.html)

## *Serial Communication*

The AUV has two PCB pods each containing an **Arduino** board. Each Arduino is connected to a battery pack. They're needed to read and to convert analogic data to digital data before sending them to the **Nvidia Jetson Xavier**. From there, data's values are sent as messages in the **ROS network**.

*More precisely:*

*Arduino board & the Jetson Xavier communicate via **USB**. To interchange data format from Arduino (C) to data format from Jetson (ROS msg), an open standard file format is needed. For this purpose, **JSON** is used. First, Arduino data is serialized into a JSON document and sent via USB. Second, when received by Jetson Xavier's port, JSON document is deserialized into a python dictionnary, which contains all Arduino's data. Finally, those data are sent as messages via specific topics to ROS network. The following diagram illustrate the communication principle used by the "power\_node":*

![](attachments/40697983/41517151.png)

# **USB Ports Protocol**

## What *Helper* & *Driver* Stand For?

As mentioned earlier, there are 2 PCB pods on the AUV, so there are two Arduino boards which will communicate via USB to the Jetson Xavier. Knowing this, two USB ports are required on the Jetson: one will be identified as the "**Helper**" and the other will be identified as the "**Driver**".

![](attachments/40697983/41517171.png)

## Helper & Driver USB Ports

First, Helper & Driver USB ports are assigned to Jetson Xavier's USB ports names. Those port names can be changed by following this tutorial [here](https://confluence.asuqtr.com/display/SUBUQTR/Associate+a+name+to+a+USB+device+in+Ubuntu).

```
DRIVER_PORT = '/dev/ttyUSB0'
```

Second, ports are opened through "open\_ports()" method, which takes care of handling serial protocol error if one of the specified bus name is not available:

```
opened_ports = open_ports([DRIVER_PORT, HELPER_PORT])
```

In this method, an "opened\_ports" dictionary is created and it returns all opened ports name.

```
opened_ports[name] = port
```

If there is no open port, the node will shut down.

## Protocol & Thread Classes

To read serial data from ASUQTR's PCB pod via Jetson Xavier's USB ports, a protocol is required. This protocol uses threads and is defined within a specialized object in the "serial.threaded" library.

```
import serial
from serial.threaded import LineReader, ReaderThread
```

Mainly, this protocol's purpose is to open USB ports and read serial binary data coming from them. Each binary package is terminated by a selected "TERMINATOR", wich in this case is "\\n", to indicate the end of the data sequence. Since data are coming from Handler port & Driver port, threads are used. When the ReaderThread is started, the selected port opens and it can receive data. If the serial port is closed or the reader loop terminated otherwise, this protocol calls the "connection\_lost" method to inform the user. Finaly, every line comming from the serial protocol is processed within a function added to the protocol by an overiding technique called **[Monkey Patching](https://www.csestack.org/monkey-patching-python-coding-example/#:~:text=Monkey%20patching%20is%20a%20dynamic%20technique%20in%20Python,to%20change%20the%20code%20inside%20class%20or%20method.).**

What are **threads**?

Threads allow multiple instructions to be executed at the same time. This allows to develop several flows in parallel instead of a single instruction flow. Power\_node requiere it because it needs to executed data wich came from 2 USB ports independently and at the same time. Click [here](https://realpython.com/intro-to-python-threading/) for more informations about threads.

# **JSON to ROS msgs**

## Pod\_Ros\_Msgs dictionary

From the JSON doc deserialization, the "json\_to\_ros\_msg" method is used to stock Arduino's data into a python dictionary called "pod\_ros\_msgs".

```
def json_to_ros_msg(data):
    json_data = json.loads(data)
    pod_ros_msgs = {}
```

Each sensor data is stored under a specific key name. This key name allows to access to data when it's required.

```
pod_ros_msgs['battery'] = BatteryState(cell_voltage=json_data["cell_voltage"])
```

Note that since there's quite few data field coming from Arduino, dictionary is a good tool, otherwise lists could have been chosen. For more informations about python dictionary, click [here](https://realpython.com/python-dicts/).

## From Dictionary To Ros Msgs

In "power\_node" method, publishers are created to publish data on specific topics. Those data are taken from "pod\_ros\_msgs" dictionary (through json\_to\_ros\_msg method) and are sent as messages on their associated topic.

```
driver_battery_pub.publish(json_to_ros_msg(data)['battery'])
```

For more specifications on the power\_node's ROS topics, see : **[ROS Topics Specifications](ROS-Topics-Specifications_40697968.html)**

# **Power\_Node Dictionaries**

## Pod\_Ros\_Msgs Dictionary's Key List

|||||
<colgroup><col /><col /><col /><col /></colgroup>|---|---|---|---|
|Key Name|Data type|Value Name|Description|
|battery|BatteryState|cell\_voltage|Values associated to the "battery" key are voltage measures of each cell of the battery pack (those values are in order: cell 1 to 4 or cell 1 to 5).|
|leak|Bool|leak\_sensor|Value associated to the "leak" key is the state of the leak sensor. If true, water is detected in the PCB pod.|
|temp|Float 32|temp\_sensor|Value associated to the "temp" key is a voltage measure associated to the temperature\*\* in the PCB pod.|

\*\*To acces to the temperature-voltage conversion table: click **[here](https://docs.google.com/spreadsheets/d/15HnTOB46jXQ3jGOYfY7FGww9bqoqdCyS1oYxoPEMCOQ/edit#gid=0)**

## Opened\_Ports Dictionary's Key List

|||||
<colgroup><col /><col /><col /><col /></colgroup>|---|---|---|---|
|Key Name|Data type|Value Name|Description|
|name|Serial class|port|Values associated to the "port" key are the USB ports names which are opened.|

## Handlers Dictionary's Key List

|||||
<colgroup><col /><col /><col /><col /></colgroup>|---|---|---|---|
|Key Name|Data type|Value Name|Description|
|DRIVER\_PORT|method|handle\_driver\_arduino|Values associated to the "DRIVER\_PORT" key are driver's ROS messages published on their associated topics.|
|HELPER\_PORT|method|handle\_helper\_arduino|Values associated to the "HELPER\_PORT" key are helper's ROS messages published on their associated topics.|

## Attachments:

![](images/icons/bullet_blue.gif) [Blank diagram (1).png](attachments/40697983/41517057.png) (image/png)  
![](images/icons/bullet_blue.gif) [Blank diagram (2).png](attachments/40697983/41517062.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_11-50-48.png](attachments/40697983/41517101.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_13-15-14.png](attachments/40697983/41517110.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_13-48-5.png](attachments/40697983/41517111.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-12-28.png](attachments/40697983/41517115.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-13-10.png](attachments/40697983/41517116.png) (image/png)  
![](images/icons/bullet_blue.gif) [Blank diagram (3).png](attachments/40697983/41517151.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-18-46.png](attachments/40697983/41517162.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-36-48.png](attachments/40697983/41517164.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-39-49.png](attachments/40697983/41517165.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-40-44.png](attachments/40697983/41517166.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-41-43.png](attachments/40697983/41517167.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-45-59.png](attachments/40697983/41517168.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-50-29.png](attachments/40697983/41517169.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_12-57-5.png](attachments/40697983/41517171.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)