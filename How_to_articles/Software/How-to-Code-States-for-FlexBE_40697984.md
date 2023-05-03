1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : How to Code States for FlexBE

Created by Unknown User (thomasg), last modified on Jan 20, 2021

*So you've done the other tutorials about getting your environment ready. Do you want to suffer, cry and code some states with FlexBE?*

![](https://media.tenor.com/images/cbd3af022801d3e2c71dae5c1b263d93/tenor.gif)

## * [Prerequisites :](#HowtoCodeStatesforFlexBE-Prerequisites:)

* [How to Make a State in Python 3](#HowtoCodeStatesforFlexBE-HowtoMakeaStateinPython3)

  * [1\. State Life Cycle](#HowtoCodeStatesforFlexBE-1.StateLifeCycle)
  * [2\. State structure](#HowtoCodeStatesforFlexBE-2.Statestructure)
  * [3\. Interfacing With the AUV's Systems](#HowtoCodeStatesforFlexBE-3.InterfacingWiththeAUV'sSystems)

    * [Publisher:](#HowtoCodeStatesforFlexBE-Publisher:)
    * [Subscriber:](#HowtoCodeStatesforFlexBE-Subscriber:)
    * [Action Client:](#HowtoCodeStatesforFlexBE-ActionClient:)
  
<a href='#HowtoCodeStatesforFlexBE-Congrats!'>Congrats!</a> <ul class='toc-indentation'> <li><a href='#HowtoCodeStatesforFlexBE-Relatedarticles'>Related articles</a></li>

# **Prerequisites :**

1. [Getting Started : Software Development](https://confluence.asuqtr.com/getting started : Software Development)

2. [How to Code with Python 3](https://confluence.asuqtr.com/display/SUBUQTR/II.+How+to+Code+with+Python+3)

3. [How to Create Virtual Machine](https://confluence.asuqtr.com/display/SUBUQTR/III.+How+to+Create+Virtual+Machine)

4. [How to install ROS and ASUQTR packages](https://confluence.asuqtr.com/display/SUBUQTR/IV.+How+to+install+ROS+and+ASUQTR+packages)

5. [How to Setup Development Environment for Missions (PC)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=36175900)**OR**[How to Setup Development Environment for Missions (Jetson)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=40697930)
6. [How to create behaviors with FlexBE](https://confluence.asuqtr.com/display/SUBUQTR/How+to+create+behaviors+with+FlexBE)
7. Pycharm IDE installed and ready

![](https://i1.wp.com/www.webprecious.com/wp-content/uploads/2019/09/Pycharm.png?fit=1350%2C500&ssl=1)

# **How to Make a State in Python 3**

Please look at states which are already implemented in the ASUQTR codebase if you need to code your a new state. This will allow you to easily adapt the current structure to your needs and document your new state as needed.

For example, if you want to code a state which uses the LQR control system, uses any of [theses states](https://confluence.asuqtr.com/s/asuqtr_states/browse/lqr_control_states/src/lqr_control_states) as a starting point to implement your idea.

## **1\. State Life Cycle**

Before we start explaining the specifics of how we need to code a state, lets get to know how they work.

Here is a useful infographic to know about the control flow of a state. It shows us in which order the state's methods are called.

![](https://i.gyazo.com/280836db64fa414ede0c38fc67476d63.png)

Key points to know are :

* All the states' \_\_init\_\_ method are called at the same time when a behavior is launched
* All the states' on\_start method are executed at the same time on behavior start. This is a good place to start behavior counter
* The on\_enter start up when we enter a state and run one time right before the execute loop (every time the states is entered)
* The execute method is called every 0.1s and should only be use for checking condition for returning outcomes
* The on\_exit method is called when the state end before the one next starts. We may use it to reset variables for the next time the state is executed
* The *on\_*stop method is called at the end of the behavior for all states OR when the behavior is preempted (be aware that every states on\_stop will happen at the same time)

## **2\. State structure**

As usual, we first need to import the modules we want to use. Here are some basic imports you might need when building states. These are essentially for the base class our state will inherit from (**EventState**), *publishers*, *subscribers*, *action servers* and *messages type*.

```
#!/usr/bin/env python3

import rospy
from flexbe_core import EventState
from rospy.exceptions import ROSInterruptException
from flexbe_core.proxy import ProxyPublisher
from flexbe_core.proxy import ProxySubscriberCached
from flexbe_core.proxy import ProxyActionClient
from std_msgs.msg import Float32, Float32MultiArray
from nav_msgs.msg import Odometry
from vision_msgs.msg import Detection2DArray, VisionInfo
from geometry_msgs.msg import Vector3
from asuqtr_control_node.msg import ControlAction, ControlGoal
from actionlib_msgs.msg import GoalStatus 
```

Next, as noted in our coding standards, constant values should be declared in all caps with underscore (also named SCREAMING\_SNAKE\_CASE)

```
HARDCODEC_VALUE_ONE = 1234
HARDCODEC_VALUE_TWO = 123456.7
```

Don't forget to write the date and your name, for FlexBE app automaticaly parses those info and uses them to meaningfully document our states.

```
'''
Created on dd.mm.yyyy

@author: YOUR_NAME_HERE
'''
```

Next, we need to **define your new** **state class** and **document our state**by using the right symbols.

It is important to follow this specific structure because FlexBE app will read these informations and show them in a friendly user way to the operator creating a behavior. When in doubt, just Copy and Paste the structure from an existing state.

Our new state will certainly have possible some parameters and outcomes (usually *success*and *failed*). We must then describe the parameters the FlexBE user must give to our state and the input/output values.

This piece of code is self-explanatory:

```
class class_state_name(EventState):
    '''
    Here you shall explain what your states does, for example "turn right with 30 degrees"...

    -- Parameter     type(int,float...)      description of the parameter   

    ># Input_value   type(int,float...)      description of the input       

    #> Output_value  type(int,float...)      description of the output      

    <= Outcome					             outcome description            

    '''
```

The first essential method is the **constructor \_\_init\_\_(self)** where we initialize and treat all of the values we documented previously. We want to instantiate most of the objects our state needs in here, because it is better if the state crashes here while it hasn't started yet. <u>This is the right place to instanciate publisher/subscribers/action\_clients</u>

This is also the method where the superclass' constructor is called, which is the place where you want to define your outcomes (see line 2).

```

    def __init__(self, parameter_1, parameter_2, parameter_3, etc): #enter all your parameters here
        super().__init__(outcomes=['outcome1', 'outcome2'])      
        self._sub = ProxySubscriberCached()
        self._sub.subscribe('ROS_topic(ex:detectnet/detections)', message_type, self._callback_example)        # subscriber example, define the ROS topic, the message type and the callback
        self._pub = ProxyPublisher({'ROS_topic(ex:control/absolute_angle)': message_type, 'ROS_topic(ex:control/linear_thrust)': Vector3})   # publisher example, define the ROS topic and the message type
       
       
        self.max_temp = parameter_1 # User gives max temperature as a parameter
		self.too_hot = False
```

The second essential method is **execute(self)**. It is repeated at a default rate of 10Hz and checks if any of the outcomes

```
    def execute(self, userdata):

        # Check if conditions are reached, if yes return one of the outcomes
		# defined in the constructor

        if self.too_hot:
            return 'failed'

        if condition_2 :
            return 'finished'

    
```

Often the best way to check on a subscriber will be here in the execute. The way you should do it is by looking if the subscriber has a received a new message and then only treating the message only then.

```
   #exemple from object_detection.py

	 # ros messages
        if self._sub.has_msg('detectnet/detections'):
            cam = self._sub.get_last_msg('detectnet/detections')

            for detection in cam.detections:
                detect_id = detection.results[0].id
                detect_score = detection.results[0].score

                # exit condition
                if self.items_labels[detect_id] == self.item_searched and detect_score >= self.min_score:
                    return 'finished'      
```

The last essential method is **on\_enter().**This function is executed once after the \_\_init\_\_() method. Usually, you would want to use this method to send a position/angle goal to the LQR control action server via the *ProxyActionClient* object from thefrom flexbe\_core.proxy module. Another common use would be to start a timer or to load the Object Detection dataset labels from the ROS parameter server.

```
	def on_enter(self, userdata):
        '''Upon entering the state, stard sending commands to control node.'''
      
      # Send a goal via ProxyActionClient or check if a subscriber received a msg.
        
```

As mentioned before the on\_enter is a great place to access and store information from the ROS parameter server, here is an example from object\_detection.py:

```
   #exemple from object_detection.py

  
 	 # get ros param database location for detection labels
        if self._sub.has_msg('detectnet/vision_info'):
            labels_location = self._sub.get_last_msg('detectnet/vision_info')

            # store objects labels from ros parameter server
            self.items_labels = rospy.get_param(labels_location.database_location)

        else:
            Logger.loginfo('ObjectDetection failed, path to labels not found in ROS param server')
            self.fail = True   
    
```

## **3. Interfacing With the AUV's Systems**

As we've discussed earlier, our state probably wants to interact with the ASUQTR AUV's different system, such as the control node, the vision node, the sensor node etc. We can do so by listening to one node (**subscriber**), sending data to a node (**publisher**) or sending a goal to a node (**action client**).

Whether we want a publisher, subscriber or action client, we should always check the [ROS Topics Specifications](ROS-Topics-Specifications_40697968.html) to know with which topic we should interact to make our desired state

* ### Publisher:

When coding a FlexBE state, you should instantiate a ROS publisher with the provided *ProxyPublisher* wrapper, see line 3.

```
def __init__(self, set_roll_degree, set_pitch_degree, set_yaw_degree, wait_for_change):
        super(angle_control_absolute, self).__init__(outcomes=['finished'])
		self._pub = ProxyPublisher({'control/absolute_angle': Vector3})
```

With the publisher instantiated, we can now publish a msg on the ROS network by calling the publish() method with the topic and msg as parameters.

As always, make sure to import the message, instanciate and fill the message. see lines 1 to 4

```
    angle = Vector3()
    angle.x = 30
    angle.y = 40
    angle.z = 0

    self._pub.publish('control/absolute_angle', angle)
```

You should be good to go on that on so lets get to the next part.

* ### Subscriber:

Again, we use the wrapper provided by FlexBE engine, the *ProxySubscriberCaches* class. This one is very usefull because it uses a caching mecanism that helps us to better manage the execution loop by avoiding callback methods. You can still use callbacks, but usually a subscriber is used to check a message's value, in which case the *ProxySubscriberCaches* ' caching mecanism is better suited for the task.

```
    def __init__(self, searched_id, minimum_score, search_time):
        '''Constructor'''
        super(object_detection, self).__init__(outcomes=['finished','failed'])
        self._sub = ProxySubscriberCached('detectnet/detections': Detection2DArray)
```

Lets use the previously defined cached subscriber **detectnet/detections** as an example. The messages received on this topic bounding boxes coordinates of detected objects in the camera stream in the form of [Detection2DArray](http://docs.ros.org/en/melodic/api/vision_msgs/html/msg/Detection2DArray.html) messages. In the execute loop, we would want to check on every loop if a message was received on the topic and if so, check if the detected object specified by the operator. If so, return the state outcome 'detected'

```
    def execute(self, userdata):
        if self._sub.has_msg('detectnet/detections'):
            msg = self._sub.get_last_msg('detectnet/detections')
			
			# Check if any of the detection is the message is the item searched
            for detection in msg.detections:
                detect_id = detection.results[0].id
                detect_score = detection.results[0].score

                # exit condition
                if self.items_labels[detect_id] == self.item_searched and detect_score >= self.min_score:
                    return 'finished'
```

* ### Action Client:

As for the action client, when we want to send a command to the AUV to influence the LQR and change the its position and angle, we will send the command and wait for a return that confirm that the position change to the one we expect.  
To do so we will need to send our command somewhat like a publisher and receive the result the same way as a subscriber in the execute.  
The following example come from increment\_roll.py:

In the \_\_init\_\_ you need to set up your \_client and your \_topic similarly to a subscriber/publisher

```
    
  def __init__(self, add_roll_degree, max_time):
        super().__init__(outcomes=['finished', 'failed'])
        self.add_roll = add_roll_degree * pi / 180
        self.max_time = rospy.Duration(max_time)
        self._topic = 'control_action_server'
        self._client = ProxyActionClient({self._topic: ControlAction})
        self.fail = False
```

In the on\_enter you will send your command using the appropriate data structure

```
    
  # Create the goal.
        goal = ControlGoal()
        goal.absolute = False
        goal.roll = self.add_roll

        try:
            self._client.send_goal(self._topic, goal)
        except Exception as e:
            Logger.logwarn('Failed to send the Control command:\n%s' % str(e))
            self.fail = True
```

The execute will look for our goal to be achieved like for a subscriber

```
    
  # Check if the action has been finished
        if self._client.has_result(self._topic):
            status = self._client.get_state(self._topic)

            # Based on the result, decide which outcome to trigger.
            if status == GoalStatus.SUCCEEDED:
                return 'finished'
            elif status in [GoalStatus.PREEMPTED, GoalStatus.REJECTED,
                            GoalStatus.RECALLED, GoalStatus.ABORTED]:
                Logger.logwarn('Navigation failed: %s' % str(status))
                self.fail = True
                return 'failed'
```

Finally to ensure that the submarine will stop moving when the state end or is aborted we need to cancel our active goal using the on\_stop

```
    
  def on_stop(self):
        self.cancel_active_goals()

    def cancel_active_goals(self):
        if self._client.is_available(self._topic):
            if self._client.is_active(self._topic):
                if not self._client.has_result(self._topic):
                    self._client.cancel(self._topic)
                    Logger.loginfo('Cancelled control_action_server active action goal.')
```

# **Congrats!**

You've made it this fat without dying or crying too much!

![](https://media3.giphy.com/media/6tHy8UAbv3zgs/giphy.gif)

![](https://media.tenor.com/images/2500c2c1e77a9229a4b0ef35db1040cb/tenor.gif)

## Related articles

* Page:

[How to work remotely using Jetbrains IDE](/display/SUBUQTR/How+to+work+remotely+using+Jetbrains+IDE)
* Page:

[How to make a pull request for FlexBE Behaviors](/display/SUBUQTR/How+to+make+a+pull+request+for+FlexBE+Behaviors)
* Page:

[How to Code States for FlexBE](/display/SUBUQTR/How+to+Code+States+for+FlexBE)
* Page:

[How to create behaviors with FlexBE](/display/SUBUQTR/How+to+create+behaviors+with+FlexBE)
* Page:

[Running/testing ROS node in Docker](/pages/viewpage.action?pageId=29851650)

## Attachments:

![](images/icons/bullet_blue.gif) [flexbe\_example\_1\_small.png](attachments/40697984/41517187.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-29\_21-25-16.png](attachments/40697984/41517201.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-44-24.png](attachments/40697984/42828001.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-45-54.png](attachments/40697984/42828002.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-47-55.png](attachments/40697984/42828003.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-49-35.png](attachments/40697984/42828004.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)