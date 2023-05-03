1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : (Deprecated)ROS Melodic installation and tutorial

Created by William Flynn, last modified by Maxime Verreault on Oct 26, 2020

This tutorial will guide the user through the installation of ROS Melodic (Robot Operating System) on their Ubuntu operating system, the process of creating a Catkin package with ROS and the development of a Python ROS node. Its goal is to speed up the learning curve for ROS by synthesizing the information from the [ROS wiki tutorials](http://wiki.ros.org/ROS/Tutorials) and the [Catkin tutorials](http://wiki.ros.org/catkin/Tutorials).

## Table of contents

* [Prerequisites](#id-(Deprecated)ROSMelodicinstallationandtutorial-Prerequisites)
* [Installing ROS Melodic and setting up a Catkin workspace](#id-(Deprecated)ROSMelodicinstallationandtutorial-InstallingROSMelodicandsettingupaCatkinworkspace)
* [Creating a Catkin package](#id-(Deprecated)ROSMelodicinstallationandtutorial-CreatingaCatkinpackage)
* [Developing a Python ROS node](#id-(Deprecated)ROSMelodicinstallationandtutorial-DevelopingaPythonROSnode)

  * [Python 3 compatibility](#id-(Deprecated)ROSMelodicinstallationandtutorial-Python3compatibility)
* [Building a ROS node](#id-(Deprecated)ROSMelodicinstallationandtutorial-BuildingaROSnode)
* [Running a ROS node](#id-(Deprecated)ROSMelodicinstallationandtutorial-RunningaROSnode)

  * [Useful ROS commands](#id-(Deprecated)ROSMelodicinstallationandtutorial-UsefulROScommands)
  * [Creating a launch file to simplify running nodes](#id-(Deprecated)ROSMelodicinstallationandtutorial-Creatingalaunchfiletosimplifyrunningnodes)

    * [Resources for creating your own launch files](#id-(Deprecated)ROSMelodicinstallationandtutorial-Resourcesforcreatingyourownlaunchfiles)
* [Conclusion](#id-(Deprecated)ROSMelodicinstallationandtutorial-Conclusion)
* [Related articles](#id-(Deprecated)ROSMelodicinstallationandtutorial-Relatedarticles)

## Prerequisites

* A computer running Ubuntu 18.04 (Recommended) ([List of supported OS](http://wiki.ros.org/melodic/Installation))

## Installing ROS Melodic and setting up a Catkin workspace

The script file available for download below contains all the required commands to install and setup ROS and a Catkin workspace on Ubuntu 18.04:

* [install\_ros.sh](attachments/17170439/17170440.sh)

After navigating into the script's directory, execute it:

```
$ sudo chmod +x install_ros.sh
$ ./install_ros.sh
```

Important information


* DO NOT RUN THE SCRIPT AS SUDO! It will execute commands with elevated privileges that some commands are not meant to be run with.
* Make sure your computer has a working internet connection. A command failing to complete successfully can break the script.
* Copying and pasting each line of the script into a terminal is a safer way to perform the installation.

If the output of the last command is:

```
$ echo $ROS_PACKAGE_PATH
/home/<your user here>/catkin_ws/src:/opt/ros/melodic/share
```

then the setup has been successful.

The commands were extracted from:

* [http://wiki.ros.org/melodic/Installation/Ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu)
* [http://wiki.ros.org/catkin/Tutorials/create\_a\_workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace)

Please refer to these links for more information about the installation process.

## Creating a Catkin package

What is Catkin? Catkin is the new build system of ROS. For more details about the role of Catkin in ROS, visit: [http://wiki.ros.org/catkin/conceptual\_overview](http://wiki.ros.org/catkin/conceptual_overview)

A Catkin package is a file structure that handles code dependencies and makes it easy to build source code.

To create a Catkin package:

* Navigate to the Catkin workspace's source directory

```
$ cd ~/catkin_ws/src
```

* Replace the tags with your desired package name and dependencies and execute the following command

```
$ catkin_create_pkg <package_name> [dependency1] [dependency2] ...
```

Here are the most common dependencies you will need for most packages:

* [rospy](http://wiki.ros.org/rospy): required for Python nodes
* [roscpp](http://wiki.ros.org/roscpp): required for C++ nodes
* [std\_msgs](http://wiki.ros.org/std_msgs): contains all the standard messages definitions
* [std\_srvs](http://wiki.ros.org/std_srvs?distro=melodic): contains all the standard services definitions

For more information, visit:[http://wiki.ros.org/catkin/Tutorials/CreatingPackage](http://wiki.ros.org/catkin/Tutorials/CreatingPackage)

## Developing a Python ROS node

Once your Catkin package is created, you can start developing some source code. All source files need to be stored in the 'src' folder of your newly created package.

ROS supports different languages for creating nodes:

* Python with [rospy](http://wiki.ros.org/rospy)
* C++ with [roscpp](http://wiki.ros.org/roscpp)
* Lisp with [roslisp](http://wiki.ros.org/roslisp)
* Java with [rosjava](http://wiki.ros.org/rosjava)
* Javascript with [roslibjs](http://wiki.ros.org/roslibjs)

For some node examples, visit:

* [http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29) (topic publishing and subscribing)

* [http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29](http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29) (service implementation and call)

Managing Linux permissions


It is important to ensure that your executable source files have the 'execute' permission in Linux. You can grant the 'execute' permission to a file by executing the following command:

```
$ sudo chmod +x <file_name>
```

### Python 3 compatibility

Python 2 has now reached end of life (EOL). It is therefore recommended to use Python 3 instead of the now deprecated Python 2. However, ROS is developed in Python 2 but is still compatible with Python 3. If you need full Python 3 compatibility, consider waiting for [ROS Noetic](http://wiki.ros.org/noetic) to be released or moving to [ROS 2](https://index.ros.org/doc/ros2/).

If you want to use Python 3, place the following at the top of any python source files (or replace similar syntax with this one):

```
#!/usr/bin/env python3
```

This character sequence is called a [shebang](https://wikipedia.org/wiki/Shebang_(Unix)). This one indicates that the source file should use Python 3 to run the code.

If you installed ROS using the provided script at the beginning, the following fixes have been implemented automatically and Catkin will use Python3 to build Python nodes.

If you encounter any compatibility issues with ROS and Python 3, refer to this article: [https://medium.com/@beta\_b0t/how-to-setup-ros-with-python-3-44a69ca36674](https://medium.com/@beta_b0t/how-to-setup-ros-with-python-3-44a69ca36674).The first step should be enough.

Changing the Catkin Python executable


It is possible to change the Catkin Python executable to use the Python 3 binaries by default, but it might affect the usage of Python 2 nodes you may find online and want to integrate to the project. In that case, you should create a seperate catkin workspace for Python 2 nodes.

## Building a ROS node

In order to build all of your Catkin packages present in your Catkin workspace:

```
$ cd ~/catkin_ws
$ catkin_make
```

The 'catkin\_make' command will compile the nodes with the required dependencies and move the generated binaries to the 'devel' (development) space.

For ROS to detect the nodes in the 'devel' space, execute the following command:

```
$ source ~/catkin_ws/devel/setup.bash
```

Sourcing the devel space


If you want ROS commands to take your nodes in consideration, this command needs to be executed in <u>every</u> new terminal and <u>every time</u> you make a significant change to the 'devel' space. (ex.: after running 'catkin\_make')

In order to automatically source the 'devel' space every time you launch a new terminal, consider writing the sourcing command to your [.bashrc](https://unix.stackexchange.com/questions/129143/what-is-the-purpose-of-bashrc-and-how-does-it-work) file with the following command:

```
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
```

Please note that the provided installation script automatically does that for you.

You are now ready to run your nodes.

## Running a ROS node

Before running a ROS node, you need to start the ROS master process in a new terminal:

```
$ roscore
```

The ROS master is the server that your nodes will register to, send messages to and receive messages from.

To execute your own node, type the following command in a terminal <u>where you previously sourced the 'devel' space</u>:

```
$ rosrun <your_package_name> <your_source_file.py> [your_parameter:=1.0]
```

### Useful ROS commands

There are many ROS commands available for you to debug your code and view the state of the ROS network:

* [rostopic](http://wiki.ros.org/rostopic): Allows you to list, view and even publish to topics from a command line interface.
* [rosmsg](http://wiki.ros.org/rosmsg?distro=melodic): Allows you list and display information about messages and their type.
* [rossrv](http://wiki.ros.org/rosmsg?distro=melodic#rossrv): Idem, but for services.

### Creating a launch file to simplify running nodes

Running multiples nodes can be a tedious operation. Opening multiple terminals, sourcing the 'devel' space and using 'rosrun' on every one of your nodes is time consuming. This is why ROS supports creating a launch file that acts like a script to automatically run the ROS master and all of the nodes you require to be executed.

A launch file:

* is an [XML](https://wikipedia.org/wiki/XML) file with the .launch extension
* is preferably stored in a /launch sub-directory of your package (ex.: my\_package/launch/my\_launch\_file.launch)
* allows you to automatically start the ROS master if it is not already running
* allows to specify ROS parameters for the execution of every node
* allows you to specify if ROS should respawn your node if it exits for any reason

Basic launch file example:

```
<launch>
  <param name="my_value" value="1" type="int" />
  <node name="talker" pkg="my_test_package" type="talker.py" output="screen" />
  <node name="listener" pkg="my_test_package" type="listener.py" output="screen" />
</launch>
```

When executed, the above launch file will:

* Start the ROS master
* Create a parameter named 'my\_value' in the [ROS parameter server](http://wiki.ros.org/Parameter%20Server). This parameter is an integer equal to 1.
* Run the 'talker.py' file from the package 'my\_test\_package' as a node named 'talker'
* Run the 'listener.py' file from the package 'my\_test\_package' as a node named 'listener'

To execute your own launch file, type the following command in a terminal <u>where you previously sourced the 'devel' space</u>:

```
$ roslaunch <your_package_name> <your_launch_file.launch>
```

#### Resources for creating your own launch files

* [http://www.clearpathrobotics.com/assets/guides/ros/Launch%20Files.html](http://www.clearpathrobotics.com/assets/guides/ros/Launch%20Files.html)
* [http://wiki.ros.org/roslaunch?distro=melodic](http://wiki.ros.org/roslaunch?distro=melodic)

## Conclusion

In conclusion, after reading and completing this tutorial, the user should be able to:

* Go through the ROS installation process on Ubuntu
* Have a basic understanding of the usage of the Catkin workspace
* Create, develop, build and run their own Python ROS node
* Debug and get meaningful information from the ROS network
* Create a launch file to speed up the startup process for multiple nodes

## Related articles

* Page:

[V.a How to Setup Development Environment for Missions (PC)](/pages/viewpage.action?pageId=36175900)
* Page:

[Running/testing ROS node in Docker](/pages/viewpage.action?pageId=29851650)
* Page:

[IV. How to install ROS and ASUQTR packages](/display/SUBUQTR/IV.+How+to+install+ROS+and+ASUQTR+packages)
* Page:

[I. Getting Started : Software Development](/display/SUBUQTR/I.+Getting+Started+%3A+Software+Development)
* Page:

[(Deprecated)ROS Melodic installation and tutorial](/display/SUBUQTR/%28Deprecated%29ROS+Melodic+installation+and+tutorial)

## Attachments:

![](images/icons/bullet_blue.gif) [install\_ros.sh](attachments/17170439/25165830.sh) (application/x-sh)  
![](images/icons/bullet_blue.gif) [install\_ros.sh](attachments/17170439/25165831.sh) (application/x-shellscript)  
![](images/icons/bullet_blue.gif) [install\_ros.sh](attachments/17170439/26607617.sh) (application/x-shellscript)  
![](images/icons/bullet_blue.gif) [install\_ros.sh](attachments/17170439/17170440.sh) (application/x-sh)

## Comments:

||
|---|
|<font>[William Flynn](https://confluence.asuqtr.com/display/~CrimsonCrow) Ca te déranges si je marque ce tuto "Deprecated" ou "Older version, no support" ?</font>![](images/icons/contenttypes/comment_16.png) Posted by Tremblayg at Aug 15, 2020 14:48|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)