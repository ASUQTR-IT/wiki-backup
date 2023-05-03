1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [Product Specifications](Product-Specifications_37355662.html)

# Sous-Marin ASUQTR : LQR Control Specifications

Created by Bastien Cote, last modified on Nov 06, 2020

||
<colgroup><col /><col /></colgroup>|:---|
|Target release|V1.0|
|Epic|[![](https://jira.asuqtr.com/images/icons/issuetypes/epic.svg)ASUQTR-30](https://jira.asuqtr.com/browse/ASUQTR-30?src=confmacro) - Asservissement In Progress|
|Document status|IN PROGRESS|
|Document owner|[Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)|
|Designer|[Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)|
|Developers|[Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)|
|QA|[Bastien Cote](https://confluence.asuqtr.com/display/~mysterfreeze)|

Hey! Welcome to the LQR control specifications. Here, you will learn how to create the mathematic model of the submarine and the mathematic behind the LQR controller.

Disclaimer


**What ever you do with the AUV**. <u>**Do not run the thrusters in the air, run them only in the water.**</u>

![](https://media3.giphy.com/media/xT3i1acWS2AQRKHgZi/giphy.gif)

**LQR Control Requirement**

## Goals

* Develop an LQR position control system for the AUV

## Background and strategic fit

We need to control the AUV in the pool to be able to move in 4 degrees of freedom without a DVL (x and y coordinates are unknown without a DVL) or 6 degrees of freedom with a DVL.

We will find all the equations needed to make the LQR controller in those 2 books :

> **HANDBOOK OF MARINE CRAFT HYDRODYNAMICS AND MOTION CONTROL (Thor I. Fossen 2011)**
>
> **&**
>
> **Computer-aided control system design (Cheng Siong Chin 2013)**

PDF Book


You can find the two PDF books in *Google Drive ASUQTR > 2020-2021 > Asservissement*

## Assumptions

* The AUV has 8 motors
* We know the mechanical properties of the AUV (inertia, mass, radius, etc)
* We will use the linear and quadratic damping calculated by another AUV similar to ours
* The AUV is controllable
* The AUV will be under water

## Requirements

|||||
<colgroup><col /><col /><col /><col /></colgroup>|---|---|---|---|
|#|Title|Importance|Notes|
|1|Define forces applied on the AUV|Must-Have|<ul><li>Note all the forces exerted on the submarine</li><li>Create a matrix for all forces</li><li>Linear and quadratic damping, rigid body and added mass inertia, Coriolis force </li></ul>|
|2|Define the motor configuration|Must-Have|<ul><li>Set the current motor configuration in a matrix</li></ul>|
|3|Create the state-space of the AUV|Must-Have|<ul><li>Generate a state-space of the AUV with the forces and the motor configuration</li></ul>|
|4|Define the LQR Equations|Must-Have|<ul><li>Note how the LQR operates (in theory)</li><li>Use a block or a library in Simulink or Python</li></ul>|
|5|Test the model in Simulink|Must-Have|<ul><li>Use the simulator Richard did (6 DOF) in Simulink and adjust the parameters</li><li>Try it with and without X and Y positions</li><li>Tune the LQR in Simulink for position control (±2 degrees on angles)</li></ul>|
|6|Transfer the equations in Python|Must-Have|<ul><li>Talk with the software team to translate to Python code and adapt it to ROS</li><li>Use a library for the LQR function</li></ul>|
|7|Test the model in the pool|Must-Have|<ul><li>Tune the LQR by doing pool testing with the AUV</li></ul>|

## User interaction and design

![](https://i.gyazo.com/60cb9790f5424cd179497f514e4b3ee4.png)

* [Goals](#LQRControlSpecifications-Goals)
* [Background and strategic fit](#LQRControlSpecifications-Backgroundandstrategicfit)
* [Assumptions](#LQRControlSpecifications-Assumptions)
* [Requirements](#LQRControlSpecifications-Requirements)
* [User interaction and design](#LQRControlSpecifications-Userinteractionanddesign)
<a href='#LQRControlSpecifications-Developments'>Developments</a> <ul class='toc-indentation'> <li><a href='#LQRControlSpecifications-Notationstandard'>Notation standard</a> <ul class='toc-indentation'> <li><a href='#LQRControlSpecifications-Motorsconfigurationonthesubmarine'>Motors configuration on the submarine</a></li> <li><a href='#LQRControlSpecifications-Motorthrustdirection'>Motor thrust direction</a></li> <li><a href='#LQRControlSpecifications-NotationoftheSocietyofNavalArchitectsandMarineEngineers(SNAME)'>Notation of the Society of Naval Architects and Marine Engineers (SNAME)</a></li> </li> <li><a href='#LQRControlSpecifications-ForcesappliedontheAUV'>Forces applied on the AUV</a> <ul class='toc-indentation'> <li><a href='#LQRControlSpecifications-MainEquations'>Main Equations</a></li> <li><a href='#LQRControlSpecifications-Inertia(M)'>Inertia (M)</a></li> <li><a href='#LQRControlSpecifications-Coriolisandcentripetal(C)'>Coriolis and centripetal (C)</a></li> <li><a href='#LQRControlSpecifications-Damping(D)'>Damping (D)</a></li> <li><a href='#LQRControlSpecifications-Gravity(g)'>Gravity (g)</a></li> <li><a href='#LQRControlSpecifications-Motorconfiguration(tau)'>Motor configuration (tau)</a></li> </ul> </li> <li><a href='#LQRControlSpecifications-StateSpace'>State Space</a></li> <li><a href='#LQRControlSpecifications-LQRequation'>LQR equation</a> <ul class='toc-indentation'> <li><a href='#LQRControlSpecifications-1.Thesystem'>1. The system</a></li> <li><a href='#LQRControlSpecifications-2.Costfunction'>2. Cost function</a></li> <li><a href='#LQRControlSpecifications-3.Ricattiequation(LQRSolving)'>3. Ricatti equation (LQR Solving)</a></li> <li><a href='#LQRControlSpecifications-Infinite-horizon,continuous-timeLQR'>Infinite-horizon, continuous-time LQR</a></li> </ul> </li> <li><a href='#LQRControlSpecifications-Conclusion'>Conclusion</a></li> </ul>

# Developments

## Notation standard

### Motors configuration on the submarine

![sub_motor.png](attachments/39223369/39223372.png)

### Motor thrust direction

![](attachments/39223382/41517064.png)

### Notation of the *Society of Naval Architects and Marine Engineers (*SNAME)

![](https://i.gyazo.com/70f6fb34ec46e7532824a3ed83e04cc8.png)

||||||
<colgroup><col /><col /><col /><col /><col /></colgroup>|---|---|---|---|---|
|DOF (Degree of freedom)|Forces and moments|Linear and angular velocities|Position and Euler angles|
|1|Motions in the x direction (surge)|X|u|x|
|2|Motions in the y direction (sway)|Y|v|y|
|3|Motions in the z direction (heave)|Z|w|z|
|4|Rotation about the x axis (roll)|K|p|φ|
|5|Rotation about the y axis (pitch)|M|q|θ|
|6|Rotation about the z axis (yaw)|N|r|Ψ|

## Forces applied on the AUV

Attach your seat belt and be ready, because this is the math part of our controller! Here we will build the AUV physics in matrix equations.

![Spongebob squarepants episode 4 season 9 GIF on GIFER - by Mezizahn](https://i.gifer.com/2XKN.gif)

### Main Equations

Here are the two mains equations that describes a 6DOF (degree of freedom) marine craft in this case an underwater AUV.

The **first one** is the equation that convert a body frame coordinates system (BODY) to a Nord-East-Down coordinates system (NED)

We will **use** this equation later to build the state space according to computer aided control book P.138

![](https://i.gyazo.com/09b3593652354bc0b9aeab56b1fba260.png)

**N** - Represent the general position (angles) of the AUV in BODY coordinates

**V** - Represent the general speed of the AUV in BODY coordinates

**N\*** - Represent the converted coordinates (NED)

State space


The symbolic state space is use to predict the future state of the submarine with the current state. So we know what will happen if we apply a certain force to the system.

We can **simplify** this equation with these singles equations (p.27 Hand book of marine craft)

![](https://gyazo.com/bbb17351b574efcedaa24a6f8edbcf1f.png)

NED stands for Nord, East, Down **velocities relative to earth** and the phi\*, theta\* and psi\* stands for the **angular velocities**in the NED coordinates system.

We can find the NED, phi\*, theta\* and psi\* with the BODY speeds and angles with the equations above.

![](https://www.mathworks.com/help/map/ned_system.png)

The **second equation** describe the forces applied on the AUV and the thrust generated by the AUV

![](https://i.gyazo.com/00b7e2081f65d300c5a9628b18a48f69.png)

**M** - Represent the rigid body and added mass inertia

**C** - Represent the Coriolis force and the centripetal force

**D** - Represent the linear and quadratic damping's

**g** - Represent the gravity and the buoyancy force

**V\*** - Represent the acceleration of the AUV

**V** - Represent the speed of the AUV

**tau** - Represent the motor combination to thrust in each 6DOF

This equation is very useful to understand what are the forces involve underwater. Note that we don't talk about perturbations yet.

### Inertia (M)

*The inertia is the propriety of matter by which matter continues in a uniform motion in a straight line, unless that state is changed by an external force.*

Underwater there is**2** types of inertia : the submarine **physical parts** inertia (rigid body inertia) and the **water** in contact with the submarine inertia (added water mass inertia) (the water propelled by the AUV movement not the motors).

The **rigid body inertia matrix** should look like this :

![](https://i.gyazo.com/651fe55dd6d21e37d587f1766e7af6a8.png)

**But!** If we say that the AUV is symmetrical in each dimensions x, y and z then we can simplify the equation like this :

![](https://i.gyazo.com/31a573e574116af6a676ef98c706e720.png)

Where to find the inertia


You can determine what are the Ix Iy Iz of the AUV with the mechanical team. They can found theses values in Solidworks.

The **added mass inertia** matrix should look like this

![](https://i.gyazo.com/dde05bb5f5d49b1650b9ee341ec95ab2.png)

We will **approximate** theses values with those equations. If we want the exact values we need to use a simulator with our AUV to find these values.

**Xu** - mass\_ratio\*mass

**Yv** - mass\_ratio\*mass

**Zw** - mass\_ratio\*mass

**Kp** - mass\_ratio\*Ix

**Mq** - mass\_ratio\*Iy

**Nr** - mass\_ratio\*Iz

Where **mass\_ratio** is :

![](https://i.gyazo.com/00dfc7ffac7fd6a82b0353b22e374aa3.png)

Additional explanations


Added (virtual) mass should be understood as pressure-induced forces and moments due to a forced harmonic motion of the body, which are proportional to the acceleration of the body.

The additional inertia of the fluid surrounding the body, which is accelerated by the movement of the body, has to be considered when an AUV is moving in a fluid.

This effect can generally be neglected in the air environment. However, in underwater applications the density of the water,*ρ*≈ 1000 kg/m3, is comparable with the density of the vehicles.

To determine what is the **total inertia** of the AUV just add these two matrix

![](https://i.gyazo.com/da4c73ec5e5c99058384ef83b1fd384a.png)

And that's it for the inertia of the AUV! ![(big grin)](images/icons/emoticons/biggrin.svg)

### Coriolis and centripetal (C)

For a rigid body moving through an ideal fluid the hydrodynamic Coriolis and centripetal matrix can be express has this matrix :

![](https://i.gyazo.com/15b9098bc699d76c8cc4ba0e1a824372.png)

**S** - Anti-symmetric matrix from a 3 elements input vector (p.20 hand book of marine craft)

**A** - Total inertia matrix (normally M)

**v1** - Linear speed of the AUV

**v2** - Angular speed of the AUV

**Anti-symmetrical matrix :**

![](https://i.gyazo.com/ff5bfb8cc30165fa85bd01387192da7f.png)

**Coriolis force gif :**

![](https://i.imgur.com/7nIYY.gif)

**Centripetal force gif :**

![](https://www.s-cool.co.uk/assets/learn_its/alevel/physics/circular-motion/angular-acceleration-and-centripetal-force/a-phy-lincir-dia01.gif)

Not a new force


It is important to understand that the**centripetal force**is**not**a fundamental**force**, but just a label given to the net**force**which causes an object to move in a circular path. Same for Coriolis.

### Damping (D)

There are **4 sources of hydrodynamic damping** for a AUV : potential damping, wave drift damping, skin friction damping and damping due to vortex shedding.

The fact that the AUV will sail at pretty **low speed** and underwater we can **neglect** wave drift damping and potential damping.

By now, we will call the skin friction damping the **linear damping** and the damping due to vortex shedding the **quadratic damping**.

We will use the values calculated by another AUV very similar to our in size. If we want the exact values we need to use a simulator with our AUV to find these values.

These two matrix should look like this :

![](https://i.gyazo.com/2fb7b7344b48d6bf16b18315beabc4b3.png)

Link


I found these values right here page 48 : [https://flex.flinders.edu.au/file/27aa0064-9de2-441c-8a17-655405d5fc2e/1/ThesisWu2018.pdf](https://flex.flinders.edu.au/file/27aa0064-9de2-441c-8a17-655405d5fc2e/1/ThesisWu2018.pdf)

Then you can add the **two matrix** and you got the D!

![](https://i.gyazo.com/94e7e824c26863e028e20c553b363f54.png)

### Gravity (g)

Don't worry were **almost done**! (Note : this part is not included in the submarine finals equations, because we assume that the submarine is **perfectly neutral** in water)

![](https://media3.giphy.com/media/3ogwG99j0ufqG5z008/giphy.gif)

If you need to add the Gravity matrix to the model here's how you can do it.

You can compute the **weight and the buoyancy force** on the AUV with these equations.

**Note** : We will approximate the AUV as a perfect sphere for the buoyancy force

![](https://i.gyazo.com/3ef199c37a379ff62fc75d0913759356.png)

![](https://i.gyazo.com/f8165f89893d32acdcda562cad64b374.png)

Then you need to know the **gravity center** position and the **buoyancy center** position in the AUV fixed frame.

Finally, you put all the values in this column matrix to find the **gravity force applied on the AUV.**

**![](https://i.gyazo.com/0546d6523bc443faa7cf7b51c4a7609f.png)**

### Motor configuration (tau)

The tau matrix represent the motor **combination to thrust** in each 6DOF. In this part we will build this matrix with the **current AUV**.

This is the motor configuration of 2019-2021. Here we can find all the **measurements** needed to make our tau matrix.

![](attachments/39223382/39223387.png)

Here is the matrix of all the**thruster position** :

**X Y Z**

![](https://i.gyazo.com/5f378857d6478d766428369c3b0ef855.png)

And the matrix of all **thruster direction** :

**X Y Z**

![](https://i.gyazo.com/1ad9d93a5be1fef28ea34bb971bc0196.png)

With these matrices we can determine the **tau** matrix with these operations :

Create an **empty 8x6 matrix** that we will call the **thrust allocation** matrix and first in this matrix copy and paste the thrust direction like this :

![](https://i.gyazo.com/49d277dfd4665791f024866f9a027ed8.png)

Then compute the XYZ**torques** (cross product of the thrust position and the thrust direction) and place it in the **last 3 columns** of the matrix like this :

![](https://i.gyazo.com/6b6f4309ff0ad16d4567785a53dc8e95.png)

After that, you need a **symbolic** value that will represent each motor in my case I called it du1 to du8 and put these values in a **column matrix** like this :

![](https://i.gyazo.com/6f6065b0b89cb63f06e0d6d4485dd837.png)

Finally **multiply** the thrust allocation matrix and the u control matrix.

The final result should **look like** this and this is the tau matrix.

![](https://i.gyazo.com/cda2d5e45087ff9e99d220d29fdbfc99.png)

This is a column matrix that map the **motor combination to throttle** in each of the 6 DOF.

There you go, the tau matrix is built. ![(smile)](images/icons/emoticons/smile.svg)

## State Space

In order to be able to **predict the motion** of the AUV depending of initial condition and present forces, we need to build the state space matrix.

According to the book computer aided control system, the **full-order state-space** functions are as follow :

This is the **order** of the system in order to know how the system will behave.

As we can see here it's a second order system, because we need the **position and the speed** of the AUV to determine the next state.

![](https://i.gyazo.com/a75f9d58e556f9ce8e3c54937bc49d87.png)

This is the state-space. The first matrix represent the matrix A multiply by the position and speed and the second matrix is the matrix B.

![](https://i.gyazo.com/fc9a1175b0af62b71359a67624702b6d.png)

![](https://i.gyazo.com/0d28578ba39f0c67117cb5f560cfaa4f.png)

Right now the system is still non-linear so, we need to linearize the systen by **passing the state space in the Jacobian matrix** and get the final A and B matrix.

**![](https://i.gyazo.com/5004bf39a58b8d742d3c995cc20b340a.png)**

Linearization


To know the basic of linearization look at this video : [https://www.khanacademy.org/math/ap-calculus-ab/ab-diff-contextual-applications-new/ab-4-6/v/local-linearization-intro](https://www.khanacademy.org/math/ap-calculus-ab/ab-diff-contextual-applications-new/ab-4-6/v/local-linearization-intro)

## LQR equation

Hey! I'm proud you made it here! Now it's time for the real control engineering part this is the complex to understand, but easy to use part.

(Easy to use, because multiple library with the LQR function exist and work really well)

![](https://media3.giphy.com/media/Y8AeLA5ZRSREY/source.gif)

Video


This video explain really well how the LQR controller work :

The LQR controller is a **mathematic algorithm** made to solve a mathematic problem by always using the **optimal solution**.

In our case, the controller is use to find the optimal solution to go from **a state** (position and speed of the AUV) to a **target state.**

**Right now**, we use the LQR to control **only the position** of the AUV.

The Linear Quadratic Regulator (LQR) can be solve in**3 steps**.

LQR Function


We will detail the LQR calculation here, but with Matlab and Python there are some control library that include an LQR function.

### 1\. The system

The first step is to determine the **A and B matrices** of the system to predict and optimize the solution for the system.

In the case of the AUV, we will use the A and B matrices found **from the state space alter the linearization.**

![](https://i.gyazo.com/fc9a1175b0af62b71359a67624702b6d.png)

We need these matrices, because the LQR controller need to predict how the AUV will react depending the present situation and the target state set to find the **optimal solution.**

Then the A and B matrices will be used in the full state feedback controller.

![](data:image/png;base64,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/v37Y9euXXZNQbskQAIkQAIkQAICCDRu3Bjvvfce2rRpI8AbusCmWGAO9O7dG507d8bIkSMFekeXSIAESIAESIAEzBKYPn061q1bhxUrVpg1xfEWEWBTbBFIK81s27bN+KkxLy8PvXr1stI0bZEACZAACZAACbhMYNWqVejXrx/Ub4dbtmzpsjecPkiATbHQXAjeMNnZ2dwxFqoR3SIBEiABEiCBcAmoHeKxY8dy4ytccA5cz6bYAciRTqF2jNUh/C1btmDAgAHo0qWL8RNleno6n2McKVSOIwESIAESIAGHCKjnEB84cABqPV+7di3effddXHvttcaX6blD7JAIYUzDpjgMWG5dqn69snLlSnz++efYuXMnDh06hDFjxiArK8stlzgvCZAACZAACZBANQQSEhJQp04dNGvWDO3atUNGRga/VCc4a9gUCxaHrpEACZAACZAACXiTQGFhIeLj473pvE+9ZlPsU+EZNgmQAAmQAAmQAAmQwDkCbIqZDSRAAiRAAiRAAiRAAr4nwKbY9ylAACRAAiRAAiRAAiRAAmyKmQMkQAIkQAIkQAIkQAK+J8Cm2PcpQAAkQAIkQAIkQAIkQAJsipkDJEACJEACJEACJEACvifAptj3KUAAJEACJEACJEACJEAC/w+khapf99J5sAAAAABJRU5ErkJggg0JkAAJkAAJ2EiA676NcMM0zaY4TGB2X86bw27CtE8CJEACJEACcghw3RekBXeK5YjBnWJZWtAbEiABEiABErCbAJtiuwmHbp87xaGzcuRK3hyOYOYkJEACJEACJCCCANd9ETIYTrAplqNFmSA8UyxMEbpDAiRAAiRAAvYR4LpvH9twLbMpDpeYzdfz5rAZMM2TAAmQAAmQgCACXPfliMGmWI4W3CkWpgXdIQESIAESIAG7CbAptptw6PbZFIfOypEreXM4gpmTkAAJkAAJkIAIAlz3RchQtjHJp0/IEcMQhGeKZQlCb0iABEiABEjARgJc922EG6ZpNsVhArP7ct4cdhOmfRIgARIgARKQQ4DrviAtuFMsRwzlyaRJkzBx4kRZTtEbEiABEiABEiABWwhw3bcFa0RGuVMcETb7BiUmJiI/Px/R0dH2TULLJEACJEACJEACrhMoLS1FcnIyCgoKXPeFDg==)

### 2\. Cost function

The second step is to determine the **Q and R matrices** to determine the cost function. **It's the Q and R matrices that will be use to tune the LQR.**

The cost function is here to determine what is the cost of the **position/speed (Q matrix)** and the cost of the **thruster (R matrix)** .

Depending on the two matrices, we can **balance** theagressivityof the control and the precision of the control.

We can have an aggressive and precis control and use the thruster more often or have a smoother control and be less precis, but use less battery power.

**The Q matrix** is a matrix with link with all 12 states individually **(x, y, z, roll, pitch, yaw, U, V, W, P, Q, R)** and we put the cost of each state in the matrix.

For example, we need a really precis roll angle, so we will put a higher value in the roll place in the matrix. Q = \[1, 1, 1, 2, 1, 1, 1, 1, 1 ,1 ,1, 1\]

**The R matrix** is a matrix link with all 8 thrusters individually **(M1, M2, M3, M4, M5, M6, M7, M8)** and we put the cost of each thruster in the matrix, but usually they are all equal.

To**tune the LQR**, we usually use a simulator or use all the sensors in the submarine**without thrusters in the air** and look at the thrusters values output from the software depending of the target state.

### 3\. Ricatti equation (LQR Solving)

### Infinite-horizon, continuous-time LQR

For the mathematic part I recommend listen to this video : [https://www.youtube.com/watch?v=wEevt2a4SKI&t=340s](https://www.youtube.com/watch?v=wEevt2a4SKI&t=340s)

This is the **cost function** :

![](attachments/39223382/42139694.png)

We can observe the Q and R matrices with the x and u matrix that represent the states and the thrusters.

First, you need to find![P](https://wikimedia.org/api/rest_v1/media/math/render/svg/b4dc73bf40314945ff376bd363916a738548d40a)is found by **solving** the continuous timealgebraic Riccati equation (n \* n symetric matrix) :

![](attachments/39223382/42139697.png)

K is **define** as shown

![](attachments/39223382/42139696.png)

The **feedback control law** that minimizes the value of the cost is (Full state feedback):

![](attachments/39223382/42139695.png)

Then we can **try each solution** yields by the Riccati equation to find the K that makes a **stable system**. (Don't do this part by hand it's really long even with a simple system)

A stable system is a system that **converge** to a definite value not the infinity.

When the matrix K that yields a stable system, we can use this value in the**full state feedback to control our system**.

## Conclusion

Thank you for reading this specification page. I hope that you found this interesting and that you have learned something about control engineering!

If you have any question just message me on discord (Bastien Côté)!

![You Rock GIFs - Get the best GIF on GIPHY](https://media.giphy.com/media/3o7abKhOpu0NwenH3O/giphy.gif)

## Attachments:

![](images/icons/bullet_blue.gif) [oie\_u6E8N3uFk4il.png](attachments/39223382/39223385.png) (image/png)  
![](images/icons/bullet_blue.gif) [moteur thrust direction.png](attachments/39223382/39223386.png) (image/png)  
![](images/icons/bullet_blue.gif) [configuration et mesure moteur.png](attachments/39223382/39223387.png) (image/png)  
![](images/icons/bullet_blue.gif) [thrust direction.png](attachments/39223382/41517064.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_22-22-46.png](attachments/39223382/42139694.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_22-25-14.png](attachments/39223382/42139695.png) (image/png)  
![](images/icons/bullet_blue.gif) [define\_K.png](attachments/39223382/42139696.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_22-30-46.png](attachments/39223382/42139697.png) (image/png)  
![](images/icons/bullet_blue.gif) [Full state feedback](attachments/39223382/42139700) (application/vnd.jgraph.mxfile)  
![](images/icons/bullet_blue.gif) [Full state feedback.png](attachments/39223382/42139701.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)