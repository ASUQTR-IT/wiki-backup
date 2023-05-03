1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Product Specifications](Product-Specifications_37355662.html)
4. [Software Specifications](Software-Specifications_42827905.html)

# Sous-Marin ASUQTR : ROS IO Node Specifications

Created by Unknown User (laurieb), last modified by Gabriel Tremblay on Nov 19, 2020

||
<colgroup><col /><col /></colgroup>|---|
|Target release|1.0|
|Task|[![](https://jira.asuqtr.com/secure/viewavatar?size=xsmall&avatarId=10318&avatarType=issuetype)ASUQTR-619](https://jira.asuqtr.com/browse/ASUQTR-619?src=confmacro) - Faire la documentation pour la io\_node Done|
|Document status|DRAFT|
|Document owner|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|Designer|[Gabriel Tremblay](https://confluence.asuqtr.com/display/~Tremblayg) [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow)|
|Developers|[Laurie B&eacute;liveau](https://confluence.asuqtr.com/display/~Laurieb)|
|QA||

* [IO Node Main Goal](#ROSIONodeSpecifications-IONodeMainGoal)
* [JETSON XAVIER GPIO](#ROSIONodeSpecifications-JETSONXAVIERGPIO)
* [Interrupt VS Polling](#ROSIONodeSpecifications-InterruptVSPolling)

# **IO Node Main Goal**

IO stands for Input/Output bus. The hardware exposes IO pins which are used to :

* Read HIGH/LOW states coming from sensors via Jetson Xavier's Input pins and send those data into messages to ROS network.

* Listen to messages on ROS network and generate corresponding HIGH/LOW states via Jetson Xavier's Output pins to control actuators.

IO are connected to various actuators & sensors which are listed further below.

*The different connections between components related to the IO\_Node are illustrated on the following diagram:*

![](attachments/41517160/41517190.png)

# **JETSON XAVIER GPIO**

The first lines that are executed in the *IO\_node* are devoted to the GPIO setup. For this, the *Jetson* library and the *GPIO* module are requiered. The following list presents all IO used until now and their corresponding purpose.

For the Jetson Xavier, when you want to declare IO pins to use, you need to declare your variables using the **TEGRA pin** name NOT the connector pin number. This is because the library uses bad mappings

Actuators/Sensors List

|||||||
<colgroup><col /><col /><col /><col /><col /><col /></colgroup>|:---:|---|:---:|:---:|:---:|:---|
|[GPIO Pin #](https://www.jetsonhacks.com/nvidia-jetson-agx-xavier-gpio-header-pinout/)|Actuators/sensors|Variable Name|[TEGRA Pin](https://docs.google.com/spreadsheets/d/1UwwBoafn2b9H-b4Ixq7Z0yi_3EQE46-BdmO1aHrK8R0/edit#gid=0)|IO|Description|
|12|Kill Switch|AUTO\_MAN\_SWITCH|DAP2\_SCLK|INPUT|High side switch controlled by a magnetic switch. This switch provides AUV's safety kill switch.|
|15|AD7997|ALERT\_TEMP|SOC\_GPIO54|INPUT|ALERT output pin on [AT30TS750A-XM8M-TCT-ND](https://www.digikey.com/en/products/detail/microchip-technology/AT30TS750A-XM8M-T/4518677) chip. Indicate temperature alarms measured by the [Thermistor](https://datasheets.globalspec.com/ds/16/DigiKey/C6275135-C682-48A7-9F2D-A8F875B72B01).|
|16|AD7997|ADC1\_BUSY|CAN1\_STB|INPUT|Digital BUSY output on [AD7997BRUZ-0-ND](https://datasheets.globalspec.com/ds/3630/DigiKey/31FCF32B-9A4D-410A-93BE-F19799C662D5) chip. Active when a conversion is in progress.|
|24|AD7997|ADC2\_BUSY|SPI1\_CS0\_N|INPUT|Digital BUSY output on [AD7997BRUZ-0-ND](https://datasheets.globalspec.com/ds/3630/DigiKey/31FCF32B-9A4D-410A-93BE-F19799C662D5) chip. Active when a conversion is in progress.|
|18|AD7997|ADC1\_CONVST|SOC\_GPIO12|INPUT|Logic Input Signal pin on [AD7997BRUZ-0-ND](https://datasheets.globalspec.com/ds/3630/DigiKey/31FCF32B-9A4D-410A-93BE-F19799C662D5) chip. This is an edge-triggered logic input. The rising edge of this signal powers up the part. The falling edge places the track/hold into hold mode and initiates a conversion.|
|21|Leak Sensor|SIGNAL\_LEAK|SPI1\_MISO|INPUT|Leak Sensor, turns high when there's water in the Main Pod.|
|23|Fan|ENABLE\_FAN|SPI1\_SCK|OUTPUT|Enables the heat sink for lowering Main Pod temperature.|
|40|Undefined|RESET\_IO|DAP2\_DOUT|OUTPUT|IO expander, [MCP23017T-E/SSCT-ND](https://www.digikey.com/en/products/detail/microchip-technology/MCP23017T-E-SS/5013524).|
|13|PCA9685|ENABLE\_PWM|SOC\_GPIO44|OUTPUT|Enables or disables the PWM output from the I2C-bus controlled 16-channel LED controller, [PCA9685](https://www.digikey.ca/en/products/detail/nxp-usa-inc/PCA9685PW-Q900-118/2406198).|
|19|Strip Led|MISSION\_LED|SPI1\_MOSI|OUTPUT|Addressable Leds, [WS2812b](https://www.amazon.ca/gp/product/B00ZHB9M6A/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&th=1). They are used to indicate in which mission the AUV is in.|

As previoulsy mentionned, IO\_node allows to send and reveived data from ROS network. To do so, **publishers** and **subscribers** are required. Input data are sent to ROS on specific topics while subscribers listen to specific topics to generate the desired outputs. For more informations on IO\_Node topics, please see [ROS Topics Specifications](ROS-Topics-Specifications_40697968.html).

# **Interrupt VS Polling**

There are two main ways to check input pin states: polling & interrupts. Polling is checking continuously for a new input state while interrupt will check for the new state only when the input pin is trigger by a edge change: *RISING, FALLING* or *BOTH*. To avoid ROS network overload, **IO\_Node is using the interrupt way to publish on topics**.

When an interrupt is created, it needs its trigger event. Input pin, type of interrupt and in IO\_Node's case, the method use to publish on the specific topic must be specified.

```
GPIO.add_event_detect(ALERT_TEMP, GPIO.BOTH, callback=interrupt_callback)
```

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-10-29\_16-9-15.png](attachments/41517160/41517182.png) (image/png)  
![](images/icons/bullet_blue.gif) [Blank diagram (4).png](attachments/41517160/41517190.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)