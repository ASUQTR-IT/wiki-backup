1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : Coding Standards

Created by William Flynn, last modified by Maxime Verreault on Oct 26, 2020

This guide describes the coding standards for the ASUQTR project.

## Table of Contents

* [Case conventions](#CodingStandards-Caseconventions)
* [Python naming standards](#CodingStandards-Pythonnamingstandards)
* [ROS naming standards](#CodingStandards-ROSnamingstandards)
* [Design tips and tricks](#CodingStandards-Designtipsandtricks)

  * [Be careful about anonymous=True when using rospy.init\_node](#CodingStandards-Becarefulaboutanonymous=Truewhenusingrospy.init_node)
  * [Consider using latch=True when publishing to a topic](#CodingStandards-Considerusinglatch=Truewhenpublishingtoatopic)

## Case conventions

Before going further, let's enumerate the case conventions that will be used in this guide :

* UpperCamelCase
* lowerCamelCase
* snake\_case
* SCREAMING\_SNAKE\_CASE

## Python naming standards

This section describes the naming standards to use in the scope of Python code. Python has it's own coding standard guide: the widely known [PEP 8](https://www.python.org/dev/peps/pep-0008/). For this project, we will adapt the naming conventions from the PEP 8 guide.

Here is a summary and adaptation of those naming conventions :

||||
<colgroup><col /><col /><col /></colgroup>|---|---|---|
|Instructions|Do|Don't|
|Modules should be named using snake\_case.|*motor\_controller.py*|*MotorController.py*|
|Class names should be named using UpperCamelCase.|```
class TargetList:
    pass
```|```
class target_list:
    pass
```|
|Methods and arguments should be named using snake\_case.|```
def motor_callback(data_in):
    pass
```|```
def motorCallback(dataIn):
    pass
```|
|Constants should be named using SCREAMING\_SNAKE\_CASE.|```
MAX_SAMPLE_FREQUENCY = 15000
```|```
max_sample_frequency = 15000
```|
|Variables should be named using snake\_case.|```
pixel_count = 0
pixel_count += 1
```|```
PIXEL_COUNT = 0
PIXEL_COUNT += 1
```|
|Private methods and variables should start with a leading underscore.|```
class TargetList:
    _my_private_var = 2

    @staticmethod
    def _my_private_static_method(i):
        return 2*i
```|```
class TargetList:
    my_private_var = 2

    @staticmethod
    def my_private_static_method(i):
        return 2*i
```|
|Throwaway variables that are unused should be named by a single underscore.|```
def on_emg_callback(data, _, _):
    print data

channel_data, flags, _, _ = unpack_struct(data_struct)

for _ in range(5):
	pass
```|```
def on_emg_callback(data, number, type):
    print data

channel_data, flags, bit, type = unpack_struct(data_struct)

for i in range(5):
	pass
```|

For all cases that would be omitted in the table above, PEP 8 will be applied.

## ROS naming standards

This section describes the naming standards to use in the scope of ROS resources. These conventions are not language dependent but examples are provided in Python. For more information, [the ROS wiki](http://wiki.ros.org/ROS/Patterns/Conventions) contains the detailed naming convention.

||||
<colgroup><col /><col /><col /></colgroup>|---|---|---|
|Instructions|Do|Don't|
|Packages should be named using snake\_case.|```
$ catkin_create_pkg submarine_robot rospy std_msgs
```|```
$ catkin_create_pkg SubmarineRobot rospy std_msgs
```|
|Nodes should be named using snake\_case and have the same name as their source file.|**motor\_controller.py**

```
rospy.init_node('motor_controller', anonymous=True)
```|**motor\_controller.py**

```
rospy.init_node('motorController', anonymous=True)
```|
|Topics and services should be named using snake\_case.|```
pub = rospy.Publisher('servo_values', Int16, queue_size=10)
```|```
pub = rospy.Publisher('servoValues', Int16, queue_size=10)
```|
|Messages should be named using UpperCamelCase.|```
pub = rospy.Publisher('sample_topic', MyCustomMessage, queue_size=5)
```|```
pub = rospy.Publisher('sample_topic', myCustomMessage, queue_size=5)
```|

## Design tips and tricks

### ![(warning)](images/icons/emoticons/warning.svg) Be careful about anonymous=True when using rospy.init\_node

```
rospy.init_node('my_node', anonymous=True)
```

The keyword argument 'anonymous' in the 'init\_node' function allows multiple instances of the same node to run with different IDs when set to True. This might not be the intended behavior.

### ![(blue star)](images/icons/emoticons/star_blue.svg) Consider using latch=True when publishing to a topic

```
pub = rospy.Publisher('my_topic', std_msgs.msg.String, queue_size=10, latch=True)
```

The keyword argument 'latch' in the 'Publisher' constructor allows the value published to a topic to be latched when set to True. A latched topic means all new nodes subscribing to it will be notified of the current latched topic value. This is especially useful for nodes publishing early while other nodes join later on.

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)