1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : How to make a pull request for FlexBE Behaviors

Created by Unknown User (thomasg), last modified on Apr 09, 2021

This article is all about making FlexBE Behaviors pull request as easy as possible.  
FlexBE being a graphical state machine editor we'll need a bit more than the automatically generated code to correctly evaluate it.  
So if you’re ready to send some Behaviors to the Master follow this guide and let’s get going.

## * [Prerequisites :](#HowtomakeapullrequestforFlexBEBehaviors-Prerequisites:)

* [1\. Ensure your Behavior is clearly readable and easy to comprehend](#HowtomakeapullrequestforFlexBEBehaviors-1.EnsureyourBehaviorisclearlyreadableandeasytocomprehend)

  * [1.1 Make a clean behavior](#HowtomakeapullrequestforFlexBEBehaviors-1.1Makeacleanbehavior)
  * [1.1 The use of Private configuration and Behavior Parameters](#HowtomakeapullrequestforFlexBEBehaviors-1.1TheuseofPrivateconfigurationandBehaviorParameters)

    * [Private Configuration](#HowtomakeapullrequestforFlexBEBehaviors-PrivateConfiguration)
    * [Behavior Parameters](#HowtomakeapullrequestforFlexBEBehaviors-BehaviorParameters)
  
<a href='#HowtomakeapullrequestforFlexBEBehaviors-2.PullrequestwithforFlexBE'>2.Pull request with for FlexBE</a> <ul class='toc-indentation'> <li><a href='#HowtomakeapullrequestforFlexBEBehaviors-2.1Startingyourpullrequest'>2.1 Starting your pull request</a></li> <li><a href='#HowtomakeapullrequestforFlexBEBehaviors-2.2Modifyingyourpullrequest'>2.2 Modifying your pull request</a></li>

# **Prerequisites :**

1. [Getting Started : Software Development](https://confluence.asuqtr.com/getting started : Software Development)

2. [How to Code with Python 3](https://confluence.asuqtr.com/display/SUBUQTR/II.+How+to+Code+with+Python+3)

3. [How to Create Virtual Machine](https://confluence.asuqtr.com/display/SUBUQTR/III.+How+to+Create+Virtual+Machine)

4. [How to install ROS and ASUQTR packages](https://confluence.asuqtr.com/display/SUBUQTR/IV.+How+to+install+ROS+and+ASUQTR+packages)

5. [How to Setup Development Environment for Missions (PC)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=36175900)**OR**[How to Setup Development Environment for Missions (Jetson)](https://confluence.asuqtr.com/pages/viewpage.action?pageId=40697930)
6. [How to create behaviors with FlexBE](https://confluence.asuqtr.com/pages/resumedraft.action?draftId=46170145&draftShareId=7d1b80ef-46e6-4bc7-9a7d-c394d52d38e5&)

# **1\. Ensure your Behavior is clearly readable and easy to comprehend**

So, before you make a pull request for your Behaviors, you need to ensure that it’s clearly readable  
and that what the Behavior is doing will be easily interpreted by any reader with basic knowledge.

## <u>1.1 Make a clean behavior</u>

Here is a perfect example of what not do:

![](https://i.gyazo.com/2ebb5594a5b694baca2dec2bfd42f04b.png)![Image result for red x](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Red_X.svg/768px-Red_X.svg.png)

Now to clean up that mess, we'll first need to space out our states and make sure our connecting arrows doesn't pass over everything else.

![](https://i.gyazo.com/e9e1c4309844219ef6274487952c6047.png)

Then we'll need to fix unreadable outcome that goes over one another. Since FlexBE doesn't allow us to move the outcome bubble on the arrow were going to have to add some comment.

Go to the top and find this button : ![](https://i.gyazo.com/fe4e660daa0aacc5f55b2d57c9466a96.png)

Write the outcome name in the comment and a mark of direction to specify which outcome go which way. (V = down, A = up, - > = right and < - = left)

![](https://i.gyazo.com/74224db6e6cd9efe82d2ae8788da69b3.png)

It is also important to ALWAYS put the outcome comments on the right side for up/down and on the bottom for left/right.

![](https://i.gyazo.com/a580d2d506731d18d873597a60929ba5.png)![](https://i.gyazo.com/a28ea358cd767fd4f12ec296f5493581.png)

Following those instructions, you will end up with a clearer sense of which outcome if present and where they are connected.

![](https://i.gyazo.com/538c64a290797e991a3082aa1069f131.png) ![Image result for arrow right](https://i.pinimg.com/originals/f7/4d/30/f74d3003e8f46b56021e42c6515a831d.jpg) ![](https://i.gyazo.com/440552959c2135241136d877491deb74.png)

Our outcomes are now a lot more readable. Now it’s time to add comments to our states as well so it’s easier for someone who doesn't exactly understand the states we use.  
Create another comment but this time explain what the objective of this particular state is.

![](https://i.gyazo.com/2d66428ace8b6ff7ffb150417b0066ff.png)

Then add it to the top of the state so we always know what state it is supposed to go with.

![](https://i.gyazo.com/8024d71373ad3c95875f4df8a289f735.png)

Next we'll also add comment next to our Behavior failed and finished outcomes.  
Create a comment, but this time mark it as important so the text is red. Then write a description of the condition that will lead to this outcome.

![](https://i.gyazo.com/7892558bbd77e068190d01764ea66d12.png)![](https://i.gyazo.com/87100322f10039e2aaa739a6b96cea59.png)![](https://i.gyazo.com/8bb31b73bafb2c6b931c9b0d308d1dd2.png)

After doing all that you will obtain a Behavior that is a lot more readable and that everyone should be able to get an idea of what's going on.

![](https://i.gyazo.com/6dd51086a29b11215b5d7f016c8fe521.png)![Image result for approval check](https://media.istockphoto.com/vectors/approval-check-icon-quality-sign-for-stock-vector-id1033823632?k=6&m=1033823632&s=612x612&w=0&h=h-YoJ4kVYB6h_9ZoaRudyJEf8nOsXwTr7lvW40YM9uU=)

## <u>1.1 The use of Private configuration and Behavior Parameters</u>

### <u>Private Configuration</u>

Now its time to talk about the handling of FlexBE Private Configuration, that you may find in the Behavior Dashboard

FlexBe Private configuration allow you to set an hardcoded value, this should be use in a behavior to simplify the making of the behavior so you can modify multiple hardcoded value at once.  
As an example, lets take the Center\_object\_in\_camera\_middle behavior. In this behavior you affect the pitch and yaw angle in multiple state and all those state also have a time limit for the angle to change.  
You do not want all the pitch and yaw values to be hardcoded, but you may use it for the maximum time to change the angle since it should not be modify in the future.

But if you ever do some testing and realize that the value you settled in the every state for the maximum time should be changed it would be annoying to do it one by one wouldn't it?

So lets simplify this a bit....

Go to your Behavior Dashboard

![](https://i.gyazo.com/d72796f362a61d60bc4ed9c6fbb18588.png)

Here you will see the Private configuration.

![](https://i.gyazo.com/bb7b281e24154c7b330f0288fa1add0f.png)

Inside it you will be able to write any variables you may want for the tuning your behavior.

![](https://i.gyazo.com/75f9216d616d2882a05d039cb01c4e37.png)

Next up go back to editing your state machine and go inside the state you want use the variable you just made.  
Inside them you simply have to write the variable name and add a minus in front if they are negative.

![](https://i.gyazo.com/2ee8eaf4caab85b3a5a04e0077d25316.png)![](https://i.gyazo.com/a50adbf21932fd3eb0b28bb1af76f22c.png)

Great were done here, your behavior should be clean and easy to understand!

### <u>Behavior Parameters</u>

Now for the Behavior Parameters.  
The behavior parameters are variables that will often change depending of the use context of the behaviors and that need to be very modular.  
Those parameters can be modify in every other behaviors you use them in and can also be change before executing them individually.

To use a behavior parameter just go to the Behavior Dashboard like for the Private configuration.

![](https://i.gyazo.com/d72796f362a61d60bc4ed9c6fbb18588.png)

Now find the Behavior Parameters

![](https://i.gyazo.com/60a555e3eb9e483d62af22e8de152371.png)

When you create a Behavior Parameters you can chose its type with the box on the left.

![](https://i.gyazo.com/b7ba219b92d0cf7cc8ae379515406bb2.png)

So chose the type of your variable and give them a name.

![](https://i.gyazo.com/16283700602e75ea29b50b51244cbcf7.png)

For each variable you also need to click the pencil on the right of them and set their basic value and their min/max.

![](https://i.gyazo.com/b02fc07d022fd38115426d0f495d4688.png)

![](https://i.gyazo.com/7c3e93f2b12e5f087793e41f3dfc34ed.png)

Once its done its time to add them to your states.  
Just to to every state you need them in and write the name of the variable with the prefix self.

![](https://gyazo.com/07501f029787d8806ab15d954b8b8c16.png)

Rinse and repeat you will now be able to configure your behavior in the Runtime control as well as in other behavior.

![](https://i.gyazo.com/7dd150277588f1ecdd49f54059338391.png)

![](https://i.gyazo.com/df23f9920f7cf57ebb74b59d18b337c2.png)

Now your behavior is even cleaner and very modular... time for the pull request!!!!

# **2.Pull request with for FlexBE**

Concerning the creation of the pull request, the code autogenerated by FlexBE is not easily readable but a normal human being.  
So we'll have to approach it a bit differently.

## <u>2.1 Starting your pull request</u>

First up we're going to launch a normal pull request on [Bitbucket](https://bitbucket.asuqtr.com/projects/SUBUQTR/repos/asuqtr_behaviors/pull-requests).

![](attachments/46170271/46170275.png)

For the next part you will need to take screenshots. Here are some great tools to helps you with that:

[Windows Snipping Tool](https://support.microsoft.com/en-us/windows/how-to-take-and-annotate-screenshots-on-windows-10-ca08e124-cc30-2579-3e55-6db63e36fbb9)

[G](https://gyazo.com/download?dl=now&lang=en)[yazo](https://gyazo.com/download?dl=now&lang=en)

Once your pull request has been created, you will need to add a comment to it that contain all the necessary photos to create the template listed bellow.

* First, you have to write the version of the screenshot concerned comment (First one wille always be Verson 1) :  
  
  Version-1
* After that write down the name of the behavior

Center\_object\_in\_camera\_middle(person) :

* Then add a screenshot of the Behavior Dashboard  
![](https://i.gyazo.com/bebaa4df2fafe36dce01465ea1e6929d.png)

* Finally add a screenshot of the Statemachine Editor with the comments and another with the Data Flow Graph and no comments  
![](https://i.gyazo.com/c97513510ea6c2ca2dc79a7c2ac89dc6.png)

**![](https://i.gyazo.com/645ab00016069d2fc1236b3e62b9de8e.png)**

**You now just have to repeat it for all the Behaviors inside your pull request!**

Your comment will look like this:

![](https://i.gyazo.com/f5941f604c7babc9a4b4f0e49cafd9d8.png)

![](https://i.gyazo.com/94ef0ea13cc7e9efc86b509818b36faf.png)

## <u>2.2 Modifying your pull request</u>

If you have to modify a pull request the first thing to do is to copy your last comment and incrment the version number by one.  
Then go through all the behaviors in your pull request and do the following :

* If the behavior is unchanged, write UNCHANGED next to its name and leave the rest as it is  
![](https://i.gyazo.com/43248ec910ec361a12bf3e34a3e39dd7.png)
* If the behavior changed since the last version, write MODIFIED next to its name and change this behaviors photos for new ones  
![](https://i.gyazo.com/5cf5e1ad0e73c22f4129ee0c89c8b606.png)

That's is post your new comment, be sure you didn't forget your commit/push and wait for the reviewers to do their damn job!

![Image result for sponge bob waiting gif](https://media4.giphy.com/media/3oKHWCXpNAmSI1RXnq/200.gif)

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

![](images/icons/bullet_blue.gif) [pull request.png](attachments/46170271/46170275.png) (image/png)

## Comments:

||
|---|
|<font>Beau travail Thomas</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Feb 19, 2021 20:21|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)