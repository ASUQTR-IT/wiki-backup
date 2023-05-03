1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Product Specifications](Product-Specifications_37355662.html)
4. [Software Specifications](Software-Specifications_42827905.html)

# Sous-Marin ASUQTR : ROS Sensor Node Specifications

Created by Unknown User (laurieb), last modified by Gabriel Tremblay on Nov 20, 2020

||
<colgroup><col /><col /></colgroup>|---|
|Target release|1.0|
|Story|[![](https://jira.asuqtr.com/images/icons/issuetypes/story.svg)ASUQTR-571](https://jira.asuqtr.com/browse/ASUQTR-571?src=confmacro) - Documentation actuator/sensor node Done|
|Document status|DRAFT|
|Document owner|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|Designer|[Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg) [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow)|
|Developers|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|QA||

## \[  \] \[ [General Abstract](#ROSSensorNodeSpecifications-GeneralAbstract) \] \[ [Sensors Node Main Goal](#ROSSensorNodeSpecifications-SensorsNodeMainGoal) \] \[ [SMBus](#ROSSensorNodeSpecifications-SMBus) \] \[ [Sensors Involved](#ROSSensorNodeSpecifications-SensorsInvolved) \] \[ [Control logic of the Sensor Node](#ROSSensorNodeSpecifications-ControllogicoftheSensorNode) \] \[ [Threads](#ROSSensorNodeSpecifications-Threads) \]

# General Abstract

## *Sensors Node Main Goal*

This ROS node purpose is to control different I2C on the Nvidia JETSON XAVIER. This node listens to 2 different sensors, MS5837 & AD7997 devices, via [I2C protocol](ROS-Actuator-Node-Specifications_42827823.html). Then, it publishes the data as messages, via topics, to ROS network.

On the following chart, you can se the Sensor Node using the I2C to publish messages to the ROS Network, while the IO Node does the same through the GPIO bus instead.

![](attachments/42827831/42827834.png)

## *SMBus*

This node uses the I2C protocol via**SMBus object** from [smbus2](https://pypi.org/project/smbus2/) python library. This object, under the hood, fetches the operating system I2C ressources and allows to read from and write to a slave address. In our case, we have 3 different addresses since we have 3 sensors. The following lines of code are read & write methods used in this node:

```
from smbus2 import SMBus
bus = SMBus(1)

data = 45
bus.write_byte_data(80,0, data)				# Write a byte to address 80, offset 0

data = [1, 2, 3, 4, 5, 6, 7, 8]
write_i2c_block_data(80, 0, data)			# Write a block of 8 bytes to address 80 from offset 0

block = read_i2c_block_data(80, 0, 16)		# Read a block of 16 bytes from address 80, offset 0
```

# Sensors Involved

|||||||
<colgroup><col /><col /><col /><col /><col /><col /></colgroup>|---|---|---|---|---|---|
|Sensor Name|Devices|Publishes on:|Protocol|I2C Address|Description|
|depth\_sensor|MS5837|[sensors/depth](ROS-Topics-Specifications_40697968.html)|I2C|0x76|MS5837 can measures pressure, altitude, depth and temperature and it communicate via I2C. For the AUV, this sensor is optimized for water depth measures.|
|adc\_sensor\_1|AD7997\_1|[sensors/motors\_vcurrents](ROS-Topics-Specifications_40697968.html)|I2C|0x21|AD7997 is a 8 channels, 10-12 bits analogic to digital converter which communicate via I2C. This ADC is meant to read each motor current (8 values). (To determine)|
|adc\_sensor\_2|AD7997\_2|[sensors/dels\_vcurrent](ROS-Topics-Specifications_40697968.html)

[sensors/ gripper\_vcurrent](ROS-Topics-Specifications_40697968.html)

[sensors/pcb\_temp1](ROS-Topics-Specifications_40697968.html)

[sensors/pcb\_temp2](ROS-Topics-Specifications_40697968.html)

[sensors/pcb\_temp3](ROS-Topics-Specifications_40697968.html)|I2C|0x22|AD7997 is a 8 channels, 10-12 bits analogic to digital converter which communicate via I2C. This ADC is meant to read DEL & Gripper current and to read PCB temperatures. (To determine)|

Note that there are two possible MS5836 models: 02BA & 30BA. The ms5837.py file is adapted for the two possibilities.

# Control logic of the Sensor Node

## Threads

What are**threads**?

Threads allow multiple instructions to be executed at the same time. Therefore, several flows in parallel can execute code instead of a single instruction flow. Sensor node is designed to use threads because the 3 different sensors operations need to be independant from one another. This way, if one of them fails, the others can continue to work corretly. Click [here](https://realpython.com/intro-to-python-threading/)for more information about threads.

This node requires the [Thread](https://docs.python.org/2/library/threading.html#thread-objects) object in **threading**library. First, thread objects are created for each sensor and stored in a list.

```
sensor_threads = [
            Thread(target=adc1_listen, args=[adc_sensor_1], daemon=True),
            Thread(target=adc2_listen, args=[adc_sensor_2], daemon=True),
            Thread(target=depth_listen, args=[depth_sensor], daemon=True)
        ]
```

Notice the **daemon** argument in the object's constructor. Daemon is a type of thread flag. One of its advantage is that the entire Python program exits when only daemon threads are left. More precisely, those type of threads can run independently in the background, so it does not affect our main program execution. Click [here](https://www.askpython.com/python-modules/daemon-threads-in-python#:~:text=%20Daemon%20Threads%20in%20Python%20%E2%80%93%20What%20Are,3%20Conclusion.%20%204%20References.%20%20More%20) for more information.

Then, each specific thread in the list can begin its activity with the start() method.

```
for thread in sensor_threads:
	thread.start()
```

Finally, the join() method allows the active thread tohold its execution until the specified thread is finished.

```
for thread in sensor_threads:
	thread.join()
```

Each sensor thread is designed to end its main loop if ROS signals it's shutdown sequence, so the Sensor Node will shutdown only if ROS signals a shutdown.

## Attachments:

![](images/icons/bullet_blue.gif) [Blank diagram (9).png](attachments/42827831/42827834.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)