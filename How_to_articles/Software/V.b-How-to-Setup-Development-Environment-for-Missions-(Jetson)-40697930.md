1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : V.b How to Setup Development Environment for Missions (Jetson)

Created by Gabriel Tremblay, last modified on Oct 26, 2020

When you want to start development of new or existing states and missions on a Jetson Xavier/nano/tx2.

** [Prerequisites](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-Prerequisites)
* [Overview](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-Overview)
* [Install ROS for ASUQTR](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-InstallROSforASUQTR)
* [Start ROS and Flexbe](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-StartROSandFlexbe)
* [Start and use the ASUQTR Dashboard](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-StartandusetheASUQTRDashboard)
* [Conclusion](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-Conclusion)
* [Related articles](#V.bHowtoSetupDevelopmentEnvironmentforMissions(Jetson)-Relatedarticles)*

## <u>Prerequisites</u>

* Your ASUQTR's Bitbucket account name and password
* A computer running Ubuntu Desktop 18.04
* NVIDIA's Jetson Xavier/nano/tx2 running L4T Ubuntu 18.04

You will see**Flexbe App** mentionned in the next section. For now, you only need to know that it is a software tool we use to design the sub's behaviors and missions.

![](attachments/40697930/40697938.png)

## <u>Overview</u>

This is the setup you want to use if you have a Jetson Xavier(or any other jetson device) and need the hardware bus for testing your states. An Ubuntu PC, called Operator Board (**OPB)**, will be running the Flexbe App GUI. The Jetson Xavier will be running ROS and ASUQTR packages.

![](attachments/40697930/40697942.png)

**Pros:**

* You can develop states and missions while the sub is in the pool. This is the main reason this setup was developed.
* You can test your states which necessitate the vision system with a webcam.
* If you need to experiment with real life sensors, but you don't have the full sub, you can plug them on the Xavier's bus (I2C, USB, GPIO, etc.) and test your states.

**Cons :**

* There may be trouble setting up Flexbe App to communicate with the Jetson.
* You need a Jetson device
* You need need to install the ASUQTR ROS environment on both machines

Since you're a clever developer, you are probably wondering "Why don't we simply run both the Flexbe App and ASUQTR - ROS on Xavier side?" **Good question.**  
We actually cannot do it because the graphical framework underneath Flexbe App is [NW.js](https://nwjs.io/), which is distributed to run on x86\_64 architecture (a normal Intel or AMD computer).

If you are skillful enough to find a way to build the framework to run on aarch64, please contact [gabriel.tremblay@uqtr.ca](mailto:gabriel.tremblay@uqtr.ca), this would be very helpful for the team.

Here are links to guides for compiling on arm32, which might be a good place to start looking : [NW.js ARMv7 binaries](https://github.com/LeonardLaszlo/nw.js-armv7-binaries)

## <u>Install ROS for ASUQTR</u>

This process is described here: [IV. How to install ROS and ASUQTR packages](IV.-How-to-install-ROS-and-ASUQTR-packages_34897922.html).

**You will need to do it on both the Jetsonand the OPB.**

## <u>Start ROS and Flexbe</u>

After installing ROS and ASUQTR packages on your Jetson, you can start the whole ASUQTR system with this command line:

```
roslaunch asuqtr_mission_executor jetson_auto_mode.launch
```

After launching, you will certainly see some errors from ROS. That is because the launch file you used is trying to launch nodes for hardware you dont necessarily have, like the Vectornav Vin100 IMU, or the PCA9685 used to communicate with motors. This will not stop your asuqtr\_mission\_executor node from running, which is the whole point of ROS.

After installing ROS and ASUQTR packages on hte **OPB**, you should now see 2 sub icons in the favorite bar. They allow to start flexbe:

![](attachments/40697930/40697933.png)

The **green** one will connect Flexbe app on the Jetson's ROS network (It will ask you the Jetson's IP and **OPB**'s root password).

![](attachments/40697930/40697936.png)

You can see "online" on the top left corner, which means the Flexbe app is correctly communicationg with the behavior engine and mirror, as indicated by the green text in the ROS terminal window.

## <u>Start and use the ASUQTR Dashboard</u>

You can see the dashboard by accessing the Jetson's IP address on you physical machine's web browser.

Next, you can simply plug in a USB controller (Xbox one, Ps4, Xbox360, Logitech, etc) and move the joysticks to interract with the sub. The joysticks will send command to the motors. You can press the **BACK/SELECT button to activate/deactivate the LQR control system.**

If Ubuntu is running on a VM, you might be prompted by Windows10 to know if you would like to plug your controller in the host (windows) or the VM(ubuntu). Just choose the one you prefer and access the ASUQTR Dashboard accordingly. If you don't, the web browser won't be able to see your controller commands

![](attachments/40697930/40697934.gif)

## <u>Conclusion</u>

**Congratulations** **!** You've successfully installed your state and mission development environment for Jetson! The next step in your learning journey shall be to follow a tutorial on how to develop states or missions on the Flexbe App.

![Nickelodeon Sticker by SpongeBob SquarePants](https://media3.giphy.com/media/qU181i0UyaVAp6uJtv/200w.gif)

## Related articles

* Page:

[V.a How to Setup Development Environment for Missions (PC)](/pages/viewpage.action?pageId=36175900)
* Page:

[III. How to Create Virtual Machine](/display/SUBUQTR/III.+How+to+Create+Virtual+Machine)
* Page:

[V.b How to Setup Development Environment for Missions (Jetson)](/pages/viewpage.action?pageId=40697930)
* Page:

[IV. How to install ROS and ASUQTR packages](/display/SUBUQTR/IV.+How+to+install+ROS+and+ASUQTR+packages)
* Page:

[I. Getting Started : Software Development](/display/SUBUQTR/I.+Getting+Started+%3A+Software+Development)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-9-10\_19-18-5.png](attachments/40697930/40697931.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-25\_14-23-59.png](attachments/40697930/40697932.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-10\_19-28-49.png](attachments/40697930/40697933.png) (image/png)  
![](images/icons/bullet_blue.gif) [auv.gif](attachments/40697930/40697934.gif) (image/gif)  
![](images/icons/bullet_blue.gif) [image2020-10-25\_15-7-16.png](attachments/40697930/40697935.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-9-10\_19-30-15.png](attachments/40697930/40697936.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-25\_15-12-18.png](attachments/40697930/40697938.png) (image/png)  
![](images/icons/bullet_blue.gif) [Setup 2.png](attachments/40697930/40697942.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)