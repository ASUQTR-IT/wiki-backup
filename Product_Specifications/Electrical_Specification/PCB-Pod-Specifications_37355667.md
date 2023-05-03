1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Product Specifications](Product-Specifications_37355662.html)
4. [Electrical Specifications](Electrical-Specifications_37355665.html)

# Sous-Marin ASUQTR : PCB Pod Specifications

Created by Maxime Verreault, last modified by Gabriel Tremblay on Nov 19, 2020

* [Battery Pod Architecture](#PCBPodSpecifications-BatteryPodArchitecture)
* [Hardware requirements](#PCBPodSpecifications-Hardwarerequirements)
* [Micro controller](#PCBPodSpecifications-Microcontroller)

  * [Application](#PCBPodSpecifications-Application)
  * [Component choice](#PCBPodSpecifications-Componentchoice)
  * [Schematic](#PCBPodSpecifications-Schematic)
* [Battery cell monitoring](#PCBPodSpecifications-Batterycellmonitoring)

  * [Theory](#PCBPodSpecifications-Theory)
  * [Component choiceS](#PCBPodSpecifications-ComponentchoiceS)
  * [Schematic](#PCBPodSpecifications-Schematic.1)
* [Leak circuit](#PCBPodSpecifications-Leakcircuit)

  * [Theory](#PCBPodSpecifications-Theory.1)
  * [Component choice](#PCBPodSpecifications-Componentchoice.1)
  * [Schematic](#PCBPodSpecifications-Schematic.2)
* [5V power supply](#PCBPodSpecifications-5Vpowersupply)

  * [Theory](#PCBPodSpecifications-Theory.2)
  * [Component choice](#PCBPodSpecifications-Componentchoice.2)
  * [Schematic](#PCBPodSpecifications-Schematic.3)
* [PCB Layout and Fabrication](#PCBPodSpecifications-PCBLayoutandFabrication)

**PCB Pods Requirements**

## Battery Pod Architecture

![](attachments/37355646/37355647.jpg)

## Hardware requirements

|||||
<colgroup><col /><col /><col /><col /></colgroup>|---|---|---|---|
|#|Title|Importance|Notes|
|1|Leak Sensor|High|<ul><li>Use Blue Robotics Design <br/><a href="https://bluerobotics.com/wp-content/uploads/2018/10/SOS-LEAK-SENSOR-SCHEMATIC.pdf" class="external-link" rel="nofollow">https://bluerobotics.com/wp-content/uploads/2018/10/SOS-LEAK-SENSOR-SCHEMATIC.pdf</a></li><li>Must send a signal to the arduino it there is a leak</li></ul>|
|2|Temperature Sensor|High|<ul><li>Must operate between -30*C and 80*C</li><li>Must be small</li><li>Use thermistor in a voltage divider configuration</li></ul>|
|3|LED Power supply|Medium|<ul><li>Need to provide 5V at 1.5A to LED strip</li><li>Use a buck converter for thermal concern</li></ul>|
|4|Cell monitoring|High|<ul><li>Avoid blowing up Arduino with higher voltage than 5V</li><li>Use voltage divider</li><li>Use a dip switch to select 4s configuration or 5s configuration</li></ul>|
|5|Arduino nano|High|<ul><li><span style="color: rgb(34,34,34);">The arduino must be easily removable</span></li><li><span style="color: rgb(34,34,34);">Use header PCB</span></li></ul>|
|6|Size|High|<ul><li>It must fit in the battery pod</li></ul>|
|7|Connectors|High|<ul><li>Every modules need to be connected and disconnected easily</li><li>Right angle</li><li>For leak sensor connectors, use the same fit as BlueRobotics leak probe</li></ul>|
|8|Mounting holes|Medium|<ul><li>Use mounting holes to secure the PCB in the pod</li><li>Use M2 hole</li></ul>|
|9|Components size|High|<ul><li>Use easy to solder component on the PCB</li><li>Use 0805 surface mount components</li></ul>|
|10|Test point|Medium|<ul><li>Use to measure the voltage level and signal</li><li>Label the test point with the signal name </li></ul>|

## Micro controller

### Application

In the pod, we need a micro controller to interact with the battery pack and the environment in the pod so we can gather data. It needs digital input-output, a 10 bits analog to digital converter and a USB connection to communicate with it.

### Component choice

||||
<colgroup><col /><col /><col /></colgroup>|---|---|---|
|#|Title|Notes|
|1|Arduino|<ul title=""><li><span style="color: rgb(0,0,0);">ATmega328 MCU</span></li><li><span style="color: rgb(0,0,0);">Small size</span></li><li><span style="color: rgb(0,0,0);">easy to program (open source)</span></li><li><span style="color: rgb(0,0,0);">USB supply and communication</span></li><li><span style="color: rgb(0,0,0);">8 bits </span></li></ul>|
|2|PIC24FJ64GA002|<ul><li>MCU only</li><li>Very small size</li><li>Intermediate programming skills</li><li>No USB communication</li><li>16bits</li></ul>|
|3|Nucleo 32|<ul><li><span style="color: rgb(51,51,51);">STM32F303K8T6 MCU</span></li><li>Small size</li><li>Intermediate programming skills</li><li>USB communication</li><li>32 bits</li></ul>|

We choose the Arduino as the micro controller, because it's USB powered, it's small size, the programming software is open source so any body can understand the code easily and the 8 bits architecture is enough for our application.

### Schematic

Arduino Nano 3D model for PCB layout.

![](https://i.gyazo.com/21b09f7e19ec93b9b5bf967414c11d08.png)

Header to mount the Arduino Nano schematic

![](attachments/37355667/37355673.png)

Has we can see in the schematic, the Arduino Nano (first image) is there just to provide a 3D model, because we wont be soldering it directly on the PCB.

We will use two headers solder on the PCB (second image) so we can change easily the Arduino Nano without desolder it.

SW1 is a 2 positions dip switch so we can select between a 4s battery or a 5s battery plugged in. There is 2 10k pull-up resistors to set high state when the switch is off.

R12 is there if we need to power the Arduino with the 9V cell, but this part is not place, because the Arduino is power via USB.

R12 is connected to VIN in the Arduino Nano. It's in do not place if we need to power the Arduino with the 9V battery cell, but basically we power the Arduino via USB.

If we look at the header closer, we can see all the net labels of the Arduino.

## Battery cell monitoring

### Theory

This part is needed to detect anomaly coming from the battery pack. We want to detect either a excessive temperature in the battery pack or voltage problem in a cell.

The battery pack can be a 4s or 5s (s = # of cells in series) and it will be selected with the help of the dip switch on the PCB.

The **thermistor** temperature-voltage table is right **[here](https://docs.google.com/spreadsheets/d/15HnTOB46jXQ3jGOYfY7FGww9bqoqdCyS1oYxoPEMCOQ/edit#gid=0)**.

For each battery pack, you can find all the voltage information in the following tables.

Voltage Divider


All the cells that can be at higher voltage than 5V and go in the ADC of the Arduino are step down with a voltage divider by a factor of 0,091 (10k with 1k divider see schematic).

**4S Battery Pack**(Drone battery pack LiPo)

|||||||||
<colgroup><col /><col /><col /><col /><col /><col /><col /><col /></colgroup>|---|---|---|---|---|---|---|---|
|Cell #|Discharged voltage (V)|Charged voltage (V)|Analog input Nano|Nano ADC input voltage discharged (V)|Nano ADC input voltage charged (V)|Nano ADC value discharged|Nano ADC value charged|
|1|3.25|4.2|A1|0.30|0.38|61|78|
|2|6.5|8.4|A2|0.59|0.76|121|156|
|3|9.75|12.6|A3|0.89|1.15|182|236|
|4|13|16.8|A4|1.18|1.53|242|313|

**5S Battery Pack** (Custom battery pack LiFe) with [http://www.phet.com.tw/en/product/view-18](http://www.phet.com.tw/en/product/view-18) Cells

|||||||||
<colgroup><col /><col /><col /><col /><col /><col /><col /><col /></colgroup>|---|---|---|---|---|---|---|---|
|Cell #|Discharged voltage|Charged voltage|Analog input Nano|Nano ADC input voltage discharged (V)|Nano ADC input voltage charged (V)|Nano ADC value discharged|Nano ADC value charged|
|1|2.7|3.6|A0|2.7|3.6|553|737|
|2|5.4|7.2|A1|0.49|0.65|100|133|
|3|8.1|10.8|A2|0.74|0.98|152|201|
|4|10.8|14.4|A3|0.98|1.31|201|268|
|5|13.5|18|A4|1.23|1.64|252|336|

### Component choiceS

We're using 4 voltage dividers 10k - 1k and if needed we can add a fifth one by replacing the two 0 ohm resistors, but we placed just the R1 0 ohm to have 3V cell voltage directly in the Arduino.

All resistor package is 0805 for easy soldering.

The connector is a JST connector with a clip so it won't fall off.

### Schematic

![](attachments/37355667/37355880.png)

This schematic is for a 5 cells battery pack. If we use the Drone battery, it's only 4 cells, so we don't use the A0 pin.

## Leak circuit

### Theory

This circuit is the same as [Bluerobotics SOS Leak sensor](https://bluerobotics.com/store/sensors-sonars-cameras/leak-sensor/sos-leak-sensor/). You can see the [schematic of the circuit right here](https://bluerobotics.com/wp-content/uploads/2018/10/SOS-LEAK-SENSOR-SCHEMATIC.pdf) from BlueRobotics. The negative current that passes in the [leak probe](https://bluerobotics.com/store/sensors-sonars-cameras/leak-sensor/sos-probes/) close (On state) the transistor when the sponge is wet and when the sponge is dry, there is not enough current to polarize the transistor so it's open (Off state) the circuit. When it's On, the LED light turn on and we have a voltage between 3.3V and 2V on the collector increasing with the degree of humidity of the sponge. When it's off the LED turn off and we have 0V on the collector of the transistor.

### Component choice

We are using the same component as BlueRobotic Leak detector to ensure the functionnality. All passive components are 0805.

||||
<colgroup><col /><col /><col /></colgroup>|---|---|---|
|QTY|Type|Name/Value|
|2|Res|1k Ω|
|1|Res|27k Ω|
|1|Res|120 Ω|
|1|Res|0 Ω|
|1|PNP transistor|[MMBT39066LT1G](https://www.onsemi.com/pub/Collateral/MMBT3906LT1-D.PDF)|
|1|Red LED|LTST-C171KRKT|

### Schematic

![](attachments/37355667/37355676.png)

## 5V power supply

### Theory

We need this external power supply to power LED strip (WS2812B LED model) with 5V with at least 1A and it need to be small to fit in the PCB Pod. For this we are using the LM22670 buck converter from Texas Instrument. It's a small footprint part with lots of features inside. The advantage of taking a TI integrated circuit is the [WeBench](https://www.ti.com/design-resources/design-tools-simulation/webench-power-designer.html) power designer. With this tool, we have all parts values and we have the layout depending the target application. We want a 5V output for the strip LED converted from the 15V cell (14.8 to 16.7V). A less important parameter is that we need a supply with less than 8 mm of height.

Important


The values generated by Webench were verified with the datasheet of the Buck to be sure there is no error.

### Component choice

||||
<colgroup><col /><col /><col /></colgroup>|---|---|---|
|QTY|Type|Name/Value|
|1|Res|75k Ω|
|1|Cap|10nF|
|1|Cap|1uF|
|2|Cap|10uF|
|3|Cap|22uF|
|1|Schottky diode|[SD2010S040S2R0](http://datasheets.avx.com/schottky.pdf)|
|1|Buck converter|[LM22670](https://www.ti.com/lit/ds/symlink/lm22670.pdf?HQS=TI-null-null-digikeymode-df-pf-null-wwe&ts=1601301627944)|
|1|Inductor|22uH|

### Schematic

![](attachments/37355667/37355881.png)

## PCB Layout and Fabrication

![](attachments/37355667/37355883.png)

Power supply layout generated by WeBench.

![](attachments/37355667/37355886.png)![](attachments/37355667/37355887.png)

## Attachments:

![](images/icons/bullet_blue.gif) [Architecture Pods.jpg](attachments/37355667/37355670.jpg) (image/jpeg)  
![](images/icons/bullet_blue.gif) [NANO.PNG](attachments/37355667/37355671.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-17\_17-1-24.png](attachments/37355667/37355672.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-17\_17-4-32.png](attachments/37355667/37355673.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-17\_19-8-56.png](attachments/37355667/37355676.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-28\_8-16-33.png](attachments/37355667/37355880.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-28\_8-56-45.png](attachments/37355667/37355881.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-28\_10-34-15.png](attachments/37355667/37355883.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-28\_11-0-52.png](attachments/37355667/37355884.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-28\_11-7-47.png](attachments/37355667/37355885.png) (image/png)  
![](images/icons/bullet_blue.gif) [tempsnip.png](attachments/37355667/37355886.png) (image/png)  
![](images/icons/bullet_blue.gif) [tempsnip2.png](attachments/37355667/37355887.png) (image/png)

## Comments:

||
|---|
|<font>[Mathieu Labranche](https://confluence.asuqtr.com/display/~Labranchem) [Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg) [Unknown User (boivinf)](https://confluence.asuqtr.com/display/~Boivinf) [Marc-Andre Morrissette-Soucy](https://confluence.asuqtr.com/display/~Soucym) Vos commentaires SVP pour se baser sur cet arcticle pour nos prochains développements merci!</font>![](images/icons/contenttypes/comment_16.png) Posted by mysterfreeze at Oct 01, 2020 13:14|
|<font>Sorry 1 mois plus tard, mais oui c'est excellent.</font>![](images/icons/contenttypes/comment_16.png) Posted by Tremblayg at Oct 25, 2020 13:21|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)