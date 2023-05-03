1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : I. Getting Started : Software Development

Created by Gabriel Tremblay, last modified on Oct 26, 2020

When joining the ASUQTR team, you probably want to learn about the various basic topics to help you start out.

* [1\. General Idea](#I.GettingStarted:SoftwareDevelopment-1.GeneralIdea)
* [2\. The AUV](#I.GettingStarted:SoftwareDevelopment-2.TheAUV)
* [3\. Basic Software Tools](#I.GettingStarted:SoftwareDevelopment-3.BasicSoftwareTools)

  * [3.1 Linux Ubuntu 18.04](#I.GettingStarted:SoftwareDevelopment-3.1LinuxUbuntu18.04)
  * [3.2 Robot Operating System (ROS)](#I.GettingStarted:SoftwareDevelopment-3.2RobotOperatingSystem(ROS))
  * [3.3 Python 3](#I.GettingStarted:SoftwareDevelopment-3.3Python3)
  * [3.4 Summing up the toolchain](#I.GettingStarted:SoftwareDevelopment-3.4Summingupthetoolchain)
* [4\. Helpful Concepts](#I.GettingStarted:SoftwareDevelopment-4.HelpfulConcepts)

  * [4.1 What is a Bus?](#I.GettingStarted:SoftwareDevelopment-4.1WhatisaBus?)
  * [4.2 What is a Virtual Machine (VM)?](#I.GettingStarted:SoftwareDevelopment-4.2WhatisaVirtualMachine(VM)?)

# 1\. General Idea

First and foremost, the ASUQTR team is composed of UQTR students working on the development of an Autonomous Underwater Vehicule (AUV). Here is the AUV:

![](attachments/37355546/37355551.png)![](attachments/37355546/37355552.png)![](attachments/37355546/37355554.png)

The goal of development is to participate in the Robosub competition from RoboNation chich is held each year in San Diego around mid July. To win the competition, you usually have to complete 4-5 missions with objectives like passing through a gate and picking up an object. Here is an picture of a team member practicing the gate mission from 2018-2019:

![](attachments/37355546/37355555.png)

If you have not seen it already, please treat yourself, sit back and relax while enjoying this master piece of a promo video made by your 2020-2021 delicious captain [Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze) :

# 2\. The AUV

The AUV has at its core a system on a chip (SoC). A SoC is a really small and complete computer. The SoC we use is NVIDIA's Jetson **Xavier**(we usually only call it **Xavier**):

![](attachments/37355546/37355557.png)![](attachments/37355546/37355558.png)![](attachments/37355546/37355559.png)

On the most right picture above, you can see the mechanical frame for the AUV with its motors (called T200 thrusters). The **Main Pod** is where the **Xavier** goes with its various electrical components, such as **Printed Circuit Boards**(PCBs), **Motor Drives**, **Inertial Measurement Unit**(IMU) sensor and much more.

The **Battery** **Pods** contain the batteries to power the AUV and each pod has an Arduino uno which communicate battery information to the **Xavier**.

# 3\. Basic Software Tools

As far as software development is concerned, there are many tools used to work with the AUV. Here is a very brief explanation of each of them. Please do not worry if it seems like a lot as a starting point, as you only need a small amount of each of them. There is no need to be a master of all.

### 3.1 Linux Ubuntu 18.04

The **Xavier** runs the **Ubuntu** **18.04**operating system which is a Linux distribution. In comparison, a macbook Pro runs OS X and a standard PC runs Windows 10.

![](attachments/37355546/37355561.png)![](attachments/37355546/37355565.png)

You will need to learn basic operations like navigating folders, executing a file, starting a program. For less advanced users, **Ubuntu** comes with a desktop environment, just like OS X or Windows 10. Here is an quick overview of the **Ubuntu** desktop environment beside OS X's:

![](attachments/37355546/37355576.png)![](attachments/37355546/37355575.png)

You can see that both are very similar, at least in looks.

### 3.2 Robot Operating System (ROS)

While working with **Ubuntu**, we use an application running on it which is called **ROS**.

![](attachments/37355546/37355566.png)

Keep in mind that even if it is called Robot *Operating system*, it is still used as a program running on the **Ubuntu** operating system. It is named as such because it behaves like an operating system: it allows us to make abstraction of hardware and inter-process communication.

This program allows us to create a communication between the many different parts of the AUV. For example, let's say we have a depth sensor. We would like to stop the thrusters when we detect that we are close to the bottom of the testing pool. How do these 2 very different pieces of hardware communicate with each other to achieve this real-life goal? Well, that is where **ROS** comes into play and listens to messages from the sensor, and when the depth reaches a certain limit (which was programmed), **ROS** sends a message to the thrusters to stop completely.

![](attachments/37355546/37355568.png)

### 3.3 Python 3

**ROS** allows our sensors to communicate with our motors and brains, but who tells **ROS** to do so? That is **YOU**the programmer who writes lines of code and tells it to listen to data or send messages. Within the ASUQTR team, the main programming language used is **Python 3**. It is one of the 2 languages officially supported by **ROS,**the second one being C++.

If you already know C++, you can stick with it and add new modules in this language, as long as you document it wisely for your teammates.

![python-logo - Skillsoft](https://www.skillsoft.com/wp-content/uploads/2018/01/python-logo.png)

There is a handful of quality resources and material to learn python on the web, like Youtube videos or Udemy courses. Here is a total beginner course for somebody with no programming background (it is also good for people with a background if you skip the first parts):

[II. How to Code with Python 3](II.-How-to-Code-with-Python-3_37355932.html)

### 3.4 Summing up the toolchain

In order to summarize all this, take a look at this brief infographic describing where software development takes place:

![](attachments/37355546/37355570.png)

You might be wondering "**How will I be able to develop software if I don't have a Jetson Xavier?"** Good question.

You simply have to have a PC running the **Ubuntu 18.04** operating system. Then, your pc acts as the Xavier in the previous picture. The main difference is that you won't be able to actually spin the motors because there are obviously none of them on your computer. However, **ROS** allows you to inspect the messages it sends to the motors, which tells you their percentage of throttle usage. Just like the motors, your PC does not havesensors (unless you use the USB ones). So **ROS** won't receive real-life data messages from the sensor. Depending on what you are working on, this might not matter or if it does, there are workarounds. Ask your senior team members for help.

# 4\. Helpful Concepts

### 4.1 What is a Bus?

After looking at the previous chapters, you now probably understand that you can program the brains and decisions of the **Xavier** to start the motors or listen to information coming from sensors. However, how do you actually use programming to produce real-life output such as spinning thrusters or lightning LEDs? This done through a **BUS**, which is basically any communication system used to transfer data from one part of a computer to another. A typical example is a USB mouse, transferring data from its laser sensor to the computer through **USB**, which stands for Universal Serial Bus.

For the AUV, at the time of writing this article, 4 bus types are used: **USB**, **I2C**, **Serial,** and **GPIO**.

In terms of programming, **Python3**offers many libraries for each of those buses, but here are some basics :

* **I2C** can be handled with "smbus2". You will also see libraries like "adafruit-blinka" which abstract some of the technicalities of **I2C** protocol if you use Adafruit's hardware. The ASUQTR AUV is actually built with Adafruit hardware, like the PCA9685 which is a PWM/servo driver for the thrusters.
* **USB** and **Serial**can be used with "serial" python module among many others. The AUV uses serial bus to communicate with the arduinos in the battery pods.
* **GPIO**is dealt with "Jetson.GPIO" python module. A use case for the AUV's GPIO is the leak sensor inside each pods. If the sensor detects too much humidity, the GPIO bus toggles from 0 to 1 to notify the rest of the ROS system.

You should always look around the internet if a code library already exists to interface with your sensor/actuator or whatever device you are adding to your computer through a bus

### 4.2 What is a Virtual Machine (VM)?

A virtual machine is the **illusion of a physical machine** as software. In other words, it is a machine residing in your physical machine (called host), thus the name "virtual" machine. Itprovide functionality of a physical computer and is totally isolated from the host on which it runs. It uses some of the host's physical ressources like RAM, processor cores and disk space.

Here is an example of a virtual machine with the Ubuntu operating system while my host is Windows 10 :

![](attachments/37355546/37355592.png)

You can see here that the software used to create and use virtual machine is called **VMware Workstation**. There other software out there that allow to use virtual machines, like **Oracle Virtual Box**. Both provide equivalent functionnalities.

Since most of people usually use a Windows 10 or Mac personal computer, **you will most likely need to use a virtual machine to install a Ubuntu 18.04 system to run ROS for ASQUTR.**

Here is a quick tutorial on how to do this for Windows 10 : [III. How to Create Virtual Machine](III.-How-to-Create-Virtual-Machine_37355602.html)

* Page:

[ASUQTR RESTful API Specification](/display/SUBUQTR/ASUQTR+RESTful+API+Specification)
* Page:

[V.a How to Setup Development Environment for Missions (PC)](/pages/viewpage.action?pageId=36175900)
* Page:

[ROS Actuator Node Specifications](/display/SUBUQTR/ROS+Actuator+Node+Specifications)
* Page:

[ROS Power Node Specifications](/display/SUBUQTR/ROS+Power+Node+Specifications)
* Page:

[ROS Sensor Node Specifications](/display/SUBUQTR/ROS+Sensor+Node+Specifications)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-8-30\_20-58-27.png](attachments/37355546/37355551.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_20-59-21.png](attachments/37355546/37355552.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_20-59-35.png](attachments/37355546/37355553.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_20-59-38.png](attachments/37355546/37355554.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-2-33.png](attachments/37355546/37355555.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-5-28.png](attachments/37355546/37355556.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-5-47.png](attachments/37355546/37355557.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-6-6.png](attachments/37355546/37355558.png) (image/png)  
![](images/icons/bullet_blue.gif) [mechanic\_sub.png](attachments/37355546/37355559.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-8-19.png](attachments/37355546/37355560.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-14-45.png](attachments/37355546/37355561.png) (image/png)  
![](images/icons/bullet_blue.gif) [ubuntulogo.png](attachments/37355546/37355564.png) (image/png)  
![](images/icons/bullet_blue.gif) [ubuntu.png](attachments/37355546/37355563.png) (image/png)  
![](images/icons/bullet_blue.gif) [ubuntulogo.png](attachments/37355546/37355562.png) (image/png)  
![](images/icons/bullet_blue.gif) [png-clipart-ubuntu-snappy-installation-canonical-package-format-no-1-text-trademark.png](attachments/37355546/37355565.png) (image/png)  
![](images/icons/bullet_blue.gif) [ros.png](attachments/37355546/37355567.png) (image/png)  
![](images/icons/bullet_blue.gif) [ros.png](attachments/37355546/37355566.png) (image/png)  
![](images/icons/bullet_blue.gif) [ros\_basic.png](attachments/37355546/37355568.png) (image/png)  
![](images/icons/bullet_blue.gif) [recap\_software.png](attachments/37355546/37355570.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-52-21.png](attachments/37355546/37355573.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-53-29.png](attachments/37355546/37355574.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-54-20.png](attachments/37355546/37355575.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-30\_21-56-0.png](attachments/37355546/37355576.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-10\_17-17-0.png](attachments/37355546/37355592.png) (image/png)

## Comments:

||
|---|
|<font>[Maxime Verreault](https://confluence.asuqtr.com/display/~niggamax24) [William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow) [Marc-Andre Morrissette-Soucy](https://confluence.asuqtr.com/display/~Soucym) [Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze) Please review, suggest obvious thing i forgot and comment on problems</font>![](images/icons/contenttypes/comment_16.png) Posted by Tremblayg at Aug 30, 2020 21:45|
|<font>Très bonne explication du software en général pour les débutants!  
On voit bien les couches d'abstractions présente dans le sous-marin.

Facile à lire</font>![](images/icons/contenttypes/comment_16.png) Posted by mysterfreeze at Aug 31, 2020 15:41|
|<font>69</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Sep 02, 2020 15:50|
|<font>Nice</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Sep 02, 2020 15:50|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)