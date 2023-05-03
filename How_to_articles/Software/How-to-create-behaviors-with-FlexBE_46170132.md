1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : How to create behaviors with FlexBE

Created by Unknown User (thomasg), last modified on Jan 20, 2021

*All right* *your starting up behavior making and have some question about how the hell your suppose to do this...  
Then your in the right place, in this tutorial were gonna go through how your suppose to use FlexBE and make Behavior.*

## * [Prerequisites :](#HowtocreatebehaviorswithFlexBE-Prerequisites:)

* [1\. What is FlexBE](#HowtocreatebehaviorswithFlexBE-1.WhatisFlexBE)  

* [2\. What are States and Behaviors](#HowtocreatebehaviorswithFlexBE-2.WhatareStatesandBehaviors)

  * [2.1 FlexBE States](#HowtocreatebehaviorswithFlexBE-2.1FlexBEStates)
  * [2.2 FlexBE Behaviors](#HowtocreatebehaviorswithFlexBE-2.2FlexBEBehaviors)
  
<a href='#HowtocreatebehaviorswithFlexBE-3.CreatingaBehaviors'>3. Creating a Behaviors</a> <ul class='toc-indentation'> <li><a href='#HowtocreatebehaviorswithFlexBE-3.1LaunchupROSandFlexBE'>3.1 Launch up ROS and FlexBE</a></li> <li><a href='#HowtocreatebehaviorswithFlexBE-3.2CreateanewsimpleBehavior'>3.2 Create a new simple Behavior</a></li> <li><a href='#HowtocreatebehaviorswithFlexBE-3.3CreateacomplexBehavior'>3.3 Create a complex Behavior</a></li> <li><a href='#HowtocreatebehaviorswithFlexBE-3.4TestingaBehavior'>3.4 Testing a Behavior</a></li> <li><a href='#HowtocreatebehaviorswithFlexBE-Congrats!'>Congrats!</a></li> <li><a href='#HowtocreatebehaviorswithFlexBE-Relatedarticles'>Related articles</a></li>

# **Prerequisites :**

1. [Getting Started : Software Development](https://confluence.asuqtr.com/getting started : Software Development)

2. [How to Code with Python 3](https://confluence.asuqtr.com/display/SUBUQTR/II.+How+to+Code+with+Python+3)

3. [How to Create Virtual Machine](https://confluence.asuqtr.com/display/SUBUQTR/III.+How+to+Create+Virtual+Machine)

4. [How to install ROS and ASUQTR packages](https://confluence.asuqtr.com/display/SUBUQTR/IV.+How+to+install+ROS+and+ASUQTR+packages)

5. [How to Setup Development Environment for Missions (PC)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=36175900)**OR**[How to Setup Development Environment for Missions (Jetson)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=40697930)

# **1\. What is FlexBE**

**![](https://i.gyazo.com/9eb5d12250d0c1726b0e88b5b77e9ab8.png)**

Basically, FlexBE is a **behaviors engine** that allows to dictate the AUV's behaviors and actions by assembling FlexBE **states**.

A **behavior** is a group of blocks(**states**) pointing to one another. It is commonly called a [state machine](https://www.itemis.com/en/yakindu/state-machine/documentation/user-guide/overview_what_are_state_machines) Here is what it looks like in FlexBE app:

## ![](attachments/46170132/46170133.png)

This assembly of states and behaviors will then allow us to use many simple actions and decisions and make more complex actions for the AUV to do.>  
All this in the ultimate goal of realizing challenge in the ROBOSUB competition.

The version of FlexBE you installed for ASUQTR is fitted for python3 and automatically downloads ASUQTR states and behaviors.

# **2\. What are States and Behaviors**

## **2.1 FlexBE States**

A FlexBE state, in the context of making behaviors, is a simple action or a decision made by the AUV.

States are represented into FlexBE by the green rectangles you use to assemble behaviors.

![](https://i.gyazo.com/6b35e9f9ca6cd06ee4b902dd6697aec4.png)

Those rectangle all have inputs and outputs that can be linked to other in order to create a chains of events that all together create a behaviors.

## **2.2 FlexBE Behaviors**

As mentioned before, FlexBE Behaviors are made of multiple states assembled together, this in the objective of executing a series of simples action that correspond to the AUV executing a complex action.

A Behavior can also be used into other behaviors and will be represented into FlexBE by red rectangles.

**![](https://i.gyazo.com/abb6c7af9fc307cf81cd4bb4fe4389fa.png)**

This way they can be used the same way a state can. Using smaller Behaviors that way, into the making of more complex ones will then allow us to not repeat work that as already been done.

This will greatly simplify the assembling and comprehension of bigger Behavior so as to not have to inspect a series of hundreds of green rectangle when looking for a bug.

# **3\. Creating a Behaviors**

In this section were going to see how a behavior is made and for that we will use the first challenge from the competition where we need to find a gate and go through it.  
In that purpose we will see how to start ROS and FlexBE, how to create a simple behavior and how to assemble more complex ones.

## **3.1 Launch up ROS and FlexBE**

**Using FlexBE on PC**

1. The first thing you need to do is to launch the ASUQTR nodes, for that use the following command in your terminal :

```
roslaunch asuqtr_mission_management pc.launch
```

2. ![](https://i.gyazo.com/d4feb63e2e1a601c16cda1ad33eca69c.png)**<------*''click''***  
  
To verify that everything is connected simply look at the top left corner of the app window for the connected status.  
  
![](https://i.gyazo.com/b2c8076c59d8521401db1252df508689.png)

**Using FlexBE with a Jetson platform**

1. As before you need to launch the ASUQTR nodes, this time with this command the Jetson terminal :

```
roslaunch asuqtr_mission_management jetson.launch
```

2. Then we need to mount the behavior and states files from the Jetson to our Ubuntu machine.

This mean that if made the behaviors on your computer while the files are not mounted you will need to back them up before and then transfer them to the Jetson.
   Now to mount the files simply click on the icon below, follow the instruction in the terminal and then simply use FlexBE normally.  
  
![](https://i.gyazo.com/8f3e1b6cd4cf0e4a5d0888063835751f.png)**<------*''click''***

## **3.2 Create a new simple Behavior**

**First of we will make a small behavior that will lather on be used in our gate challenge behavior.**

So to begin, launch your FlexBE and click on New Behavior in the top middle.

![](https://i.gyazo.com/3f29e07c1450d13b0ca81d928f869160.png)

![](attachments/46170132/46170134.png)

Make sure asuqtr\_behaviors and **fill all the following info.**

![](https://i.gyazo.com/d3a87cb7c53a9929418b053268b726de.png)

Click "*Statemachine Editor"* and *"Add State"*

*![](https://i.gyazo.com/b57d6d1436e6d5c095f66093d78dbdf2.png)*

*![](https://i.gyazo.com/1ba823b92e85f0434a2f024669b17a74.png)*

![](attachments/46170132/46170135.png)

This will open the **Add New State** right panel, in which we can select select the package from which the state we want come from (camera, lqr, miscellaneous etc...).  
Then we click the behavior we want to add, enter a name in the top of the panel and finally click on add.

![](https://i.gyazo.com/0c2d2320b86535e9249c18eca2c92a84.png)

This will add a state block to our behavior and we can click on the state block to access is parameters panel.

![](https://i.gyazo.com/6b35e9f9ca6cd06ee4b902dd6697aec4.png)

In the parameter panel you can precise what you want from this state.  
In this case we want it to look for a gate with a minimum score of 70% and in a maximum of 5 second.  
If you do not know what the parameter mean simply mouse over it and a box with a description will pop up.

![](https://i.gyazo.com/d580ad402df4ff49af4b6a67617a707e.png)

Now that the parameters a filled you should link your state to the other. To do that start by clicking the root dot in the top left and link it to your first state.

![](https://i.gyazo.com/196381d61013ffdae0f0ef670b52dada.png)

Then you need to link the outcome of your states to where they need to go, as an example the outcome our look\_for\_object behavior as two outcome : finished and failed.

If it finish it mean that the gate as been found and that our small behavior should end. Else it it failed we should try to turn to the right and see if we can find it again.

Our first behavior will then look like this :

![](https://i.gyazo.com/46faced06d3e94ec9de2437cca287073.png)

That wraps it up for our first behavior, we now only have to save it and test it.

![](https://i.gyazo.com/24c499beceeca8acabc722f858e01a19.png)

## **3.3 Create a complex Behavior**

**Here we will see how to assemble more complex behavior by assembling a mix of states and other behavior.**  
**This will be where we make the behavior corresponding to the gate challenge of the competition.**

To start up we will add our turn\_and\_find behavior we just made, we do this just like we add a state.

**![](https://i.gyazo.com/80ba34d4414e1462d70c748169305bc4.png)**

**![](https://i.gyazo.com/abb6c7af9fc307cf81cd4bb4fe4389fa.png)**

Those block are just like sate blocks and need to be connected with others. For our complete behavior this will look like this:

![](https://i.gyazo.com/56556d4eccf99a34f0e6b3a685f759fc.png)

You can also fine tune every behavior by double clicking them to see what's inside of it.

![](https://i.gyazo.com/e274e2118abc62adf7d98401f4515388.png)

By double clicking our find\_and\_turn behavior we found back the one we made before. You can go back to your complete behavior by clicking its name right next to the root dot.

![](https://i.gyazo.com/d7fecf4c4e112cc2b423d15d68e1d7e3.png)

If you wish to see the rest of the behavior here it is:

![](attachments/46170132/46170144.gif)

As you can see Behavior making can stack up very quickly and simple behavior become part of greater one's.

## **3.4 Testing a Behavior**

To try and run your behavior, click on *"Runtime Control"*and **Start** **Execution,**while the behavior is selected, else you just have to load it.

![](https://i.gyazo.com/f18c734e07f3dc876261a8002dd2d3db.png)

![](https://i.gyazo.com/c312d1d26ce00c657c44e61e52e67794.png)

To see the result of the execution simply write the ip (192.168.X.XX) of you machine into your web browser and you will see ASUQTR web page and the motors forces.

![](https://i.gyazo.com/c2ec93b13c74bb792bc1e54e2be3ea79.png)

To see the camera in action simply write the same ip in the browser but ad :8080 at the end

![](https://i.gyazo.com/a2f492e54b4cf81917d31a0a26b2b3cf.jpg)

## **Congrats!**

You now know the basics of Behavior making, now you can go and make some yourself!

![Spongebob thumbs up good job GIF on GIFER - by Munilore](https://i.gifer.com/1wn.gif)

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

![](images/icons/bullet_blue.gif) [flexbe\_example\_1\_small.png](attachments/46170132/46170133.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-44-24.png](attachments/46170132/46170134.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-45-54.png](attachments/46170132/46170135.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-47-55.png](attachments/46170132/46170136.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-26\_23-49-35.png](attachments/46170132/46170137.png) (image/png)  
![](images/icons/bullet_blue.gif) [491516ef4db3562edb56f046b0dd9549.mp4](attachments/46170132/46170142.mp4) (video/mp4)  
![](images/icons/bullet_blue.gif) [image2021-1-20\_15-57-21.png](attachments/46170132/46170143.png) (image/png)  
![](images/icons/bullet_blue.gif) [ezgif.com-gif-maker (2).gif](attachments/46170132/46170144.gif) (image/gif)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)