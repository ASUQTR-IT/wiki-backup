1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)

# Sous-Marin ASUQTR : How to create parts in Altium Designer

Created by Bastien Cote, last modified on Jan 24, 2021

* [What is this tutorial?](#HowtocreatepartsinAltiumDesigner-Whatisthistutorial?)
* [What you need :](#HowtocreatepartsinAltiumDesigner-Whatyouneed:)
* [Step 1 : Software](#HowtocreatepartsinAltiumDesigner-Step1:Software)

  * [Altium Designer](#HowtocreatepartsinAltiumDesigner-AltiumDesigner)
  * [PCB Library](#HowtocreatepartsinAltiumDesigner-PCBLibrary)
* [Find your parts](#HowtocreatepartsinAltiumDesigner-Findyourparts)
* [Create the symbol in Altium](#HowtocreatepartsinAltiumDesigner-CreatethesymbolinAltium)
* [Create the footprint](#HowtocreatepartsinAltiumDesigner-Createthefootprint)
* [Link footprint and symbol](#HowtocreatepartsinAltiumDesigner-Linkfootprintandsymbol)
* [All done!](#HowtocreatepartsinAltiumDesigner-Alldone!)

Ho, hi! Nice music here : [https://www.youtube.com/watch?v=lu\_ziQY9bSs&list=RDlu\_ziQY9bSs&start\_radio=1](https://www.youtube.com/watch?v=lu_ziQY9bSs&list=RDlu_ziQY9bSs&start_radio=1)

![](https://i.pinimg.com/originals/d3/87/d5/d387d5ba991ce78f034f96ca32fcc9ed.gif)

## What is this tutorial?

This tutorial will guide you to find on the internet and create the part that you need in Altium Designer to make a printed circuit board (PCB).

## What you need :

* Altium Designer
* Altium Designer ASUQTR license
* PCB Library (see below how to download)

## Step 1 : Software

There is two main software we will use to create the parts we need to develop the PCB.

### Altium Designer

At this point you probably know that Altium is the software to do schematics and design the PCB so all you need a licence.

ASUQTR have a couple of Altium Licences. You need to ask the electronic manager to have a Altium License.

### PCB Library

PCB Library is a software used to create footprint easily for most standard parts (through hole and surface mount).

First thing you, need to download PCB Library by following theses steps.

1. Go to [https://www.pcblibraries.com/](https://www.pcblibraries.com/)
2. click on ![](attachments/46170155/46170156.png)
3. click on ![](attachments/46170155/46170157.png)
4. Create an account and login in
5. click on ![](attachments/46170155/46170158.png)
6. Accept the EULA
7. A download should start
8. Unzip the file and click the .exe file
9. Follow the installation steps
10. Go to PCBLibrary, login in and click to see your profil ![](attachments/46170155/46170159.png)
11. Select the free pro cad format and note the key ![](attachments/46170155/46170160.png)
12. Launch PCB Library
13. Enter your email and the key ![](attachments/46170155/46170161.png)
14. The installation of the software is complete good job!

## Find your parts

When you know what type of circuit you want to do, you need to find the parts that will compose the circuit. For this the website Digikey is really helpful.

Digikey is like Canadien Tire, but for electornic parts. You can find almost everything you need with really fast shipping (next business day).

Perhaps, you need to be really specific when you search on this website, because there is a really wide range of electronic parts.

Lets search a 1k resistor 0805 +-1% surface mount.

1. Go to [https://www.digikey.com/](https://www.digikey.com/)
2. Enter "res" in the search bar
3. click on chip resistor![](attachments/46170155/46170162.png)
4. First thing **ALWAYS SEARCH FOR IN STOCK AND ACTIVE PART![](attachments/46170155/46170163.png)**
5. Then you can specify in the search tool the following parameters : Value = 1k, Tolerance = 1%, Package/case = 0805(2012)
6. click on ![](attachments/46170155/46170164.png)
7. You can see that there is still 51 results so we will specify more things
8. Packaging = cut tape, power dissipation = 1/4 watt and manufacturer = yageo
9. Now we should be down to 2 one automotive resistor (not needed in our application) so we will chose the other resistor the 13-RC0805FR-7W1KLCT-ND
10. Good job you have found your first part on digikey!

## Create the symbol in Altium

To use a component in Altium, you need to**create a symbol**to place in your schematic. For this part I will assume that you have done the**first tutorial of Altium**.

The first step is to open Altium Designer and open a schematic library. The library can be in a project or stand alone this is no matter for the exercise, but usually**it is for a specific project.**

Here I have my project open with the **schematic library**.

![](attachments/46170155/46170169.png)

You have to create a new component and **name it** with the following form : Manufacturer\_ManufacturerPartNumber

**Example** : Yageo\_RC0805FR-7W1KL

For each parts, we need to add parameters and specification.

All passive components should be like this :

![](attachments/46170155/46170170.png)

Most of the parameters you see up here are from the Digikey page of the part.

All other components like connectors, integrated circuit or other should follow the next form :

![](attachments/46170155/46170171.png)

**BEWARE : The Designator should match the type of component you are using (example a capacitor is C?) see : [https://en.wikipedia.org/wiki/Reference\_designator](https://en.wikipedia.org/wiki/Reference_designator)**

[https://en.wikipedia.org/wiki/Reference\_designator](https://en.wikipedia.org/wiki/Reference_designator)

Tips


In Altium you can make a first component and copy/paste the component to spend less time creating Parameters

Once the parameters are all set, we can draw the part we are using.

Example 1 : **Resistor**

**Place 2 passives pins (1 and 2) and draw the resistor symbol in blue with the drawing tool.**

![](attachments/46170155/46170172.png)

Example 2 : **Integrated circuit**

**Place each pin with the name referred from the datasheet and specify the pin type (example Power, Passive, input, output)**

**![](attachments/46170155/46170174.png)**

Keep in mind


Keep in mind that your symbol should be reusable in each project. For example a connector, name the pins 1,2,3...,n and not the name that you are using in the project. This way you will be able to reuse it.

## Create the footprint

Now that the symbol is done, we will do the**footprint**.

Open **PCB Library**and the **datasheet** of the component you are using at the page where you can find the **mechanical specs** of the component.

In PCB Library you have to select the type of component you have. In this case, we have a surface mount resistor.

So select on top left surface mount :

![](attachments/46170155/46170177.png)

And we got a chip resistor so select the chip :

![](attachments/46170155/46170178.png)

We will select the resistor family.

Has you can see we need the mechanical dimensions of the chip, so we will open the datasheet of our resistor.

For the resistor you can open the datasheetat page 4. The datasheet is available from Digikey.

You will find**this table** :

![](attachments/46170155/46170175.png)

![](attachments/46170155/46170176.png)

We have a RC0805 so we will look at the RC0805 line to see its mechanical specs.

Enter the mechanical specs in PCB Library and click **OK** :

**![](attachments/46170155/46170179.png)**

Now the footprint created we have to import it in Altium.

click this button in PCB Library to export :

**![](attachments/46170155/46170181.png)**

A window should pop. You have to do a couples things to be able to **export it properly**.

1. Select **Altium Designer as translator**
2. Set the target library as **use existing**
3. Set a **output directory** where you can find the footprint easily for both the footprint and the 3D model

At the end the window should look like this :

![](attachments/46170155/46170183.png)

Then, click on create and close.

The last step to import it to Altium Designer.

You need to be in your footprint library first. Like this :

![](attachments/46170155/46170184.png)

![](attachments/46170155/46170185.png)

Then click on the top left **File → Run script**

A window like this should appear :

![](attachments/46170155/46170186.png)

Click on browse and select the footprint you just generated with PCB Library.

The window should look like this now :

![](attachments/46170155/46170187.png)

Double click on Create a library and your done! The footprint should look like this :

![](attachments/46170155/46170188.png)

## Link footprint and symbol

Go to your symbol in the schematic library and click on your component.

![](attachments/46170155/46170191.png)

Click on add footprint and select the PCB Library where your footprint is to link it with the symbol.

![](attachments/46170155/46170192.png)

That's it!

## All done!

Good job you just finished the tutorial to search and create the part you need in Altium Designer. You should be able to use the part in your projects.

## Attachments:

![](images/icons/bullet_blue.gif) [image2021-1-23\_23-28-58.png](attachments/46170155/46170156.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-29-40.png](attachments/46170155/46170157.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-31-55.png](attachments/46170155/46170158.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-37-32.png](attachments/46170155/46170159.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-40-35.png](attachments/46170155/46170160.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-44-5.png](attachments/46170155/46170161.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-55-36.png](attachments/46170155/46170162.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-56-48.png](attachments/46170155/46170163.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-23\_23-59-22.png](attachments/46170155/46170164.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_16-56-25.png](attachments/46170155/46170169.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-1-32.png](attachments/46170155/46170170.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-3-38.png](attachments/46170155/46170171.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-7-0.png](attachments/46170155/46170172.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-8-6.png](attachments/46170155/46170173.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-8-26.png](attachments/46170155/46170174.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-15-38.png](attachments/46170155/46170175.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-16-38.png](attachments/46170155/46170176.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-18-37.png](attachments/46170155/46170177.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-19-31.png](attachments/46170155/46170178.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-23-6.png](attachments/46170155/46170179.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-23-22.png](attachments/46170155/46170180.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-24-11.png](attachments/46170155/46170181.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-24-27.png](attachments/46170155/46170182.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_17-27-46.png](attachments/46170155/46170183.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-35-39.png](attachments/46170155/46170184.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-36-2.png](attachments/46170155/46170185.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-37-12.png](attachments/46170155/46170186.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-38-43.png](attachments/46170155/46170187.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-39-19.png](attachments/46170155/46170188.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-39-33.png](attachments/46170155/46170189.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-40-37.png](attachments/46170155/46170190.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-41-3.png](attachments/46170155/46170191.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-1-24\_18-42-15.png](attachments/46170155/46170192.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)