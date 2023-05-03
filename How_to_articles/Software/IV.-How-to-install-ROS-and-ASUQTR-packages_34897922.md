1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Software](42827871.html)

# Sous-Marin ASUQTR : IV. How to install ROS and ASUQTR packages

Created by Gabriel Tremblay, last modified on Oct 26, 2020

*When you want to setup a ROS development environment and start coding for ASUQTR packages*

** [Requirements:](#IV.HowtoinstallROSandASUQTRpackages-Requirements:)
* [1\. Download ROS for ASUQTR](#IV.HowtoinstallROSandASUQTRpackages-1.DownloadROSforASUQTR)
* [2\. Build ROS and ASUQTR packages](#IV.HowtoinstallROSandASUQTRpackages-2.BuildROSandASUQTRpackages)
* [Conclusion](#IV.HowtoinstallROSandASUQTRpackages-Conclusion)
* [Related articles](#IV.HowtoinstallROSandASUQTRpackages-Relatedarticles)*

## <u>Requirements:</u>

1. A real/virtual machine (usually a PC or NVIDIA's Jetson Xaxier) running Ubuntu 18.04 operating system.  

   1. Other Debian based Linux distros might work but have not been tested. Contact [gabriel.tremblay@uqtr.ca](mailto:gabriel.tremblay@uqtr.ca) or create a ticket at [https://jira.asuqtr.com/servicedesk/customer/portal/1](https://jira.asuqtr.com/servicedesk/customer/portal/1) for troubleshooting.
2. ASUQTR's Bitbucket username and password

## <u>1\. Download</u><u>ROS for ASUQTR</u>

First, on your desktop, open a terminal by typing **Ctrl+Alt+T** or by searching "Terminal" in the Activities search box.

![](attachments/34897922/34897924.png)

![](attachments/34897922/34897925.png)

Then, one by one, copy the following command lines and paste them into the terminal. Shortcut for pasting in Ubuntu terminal is **Ctrl+Shift+V,** you can also right-click the terminal and select "Paste".

```
sudo apt install git
cd ~/ 
git clone https://bitbucket.asuqtr.com/scm/subuqtr/ros_workspace.git
```

The first line uses "*apt install*"which is the common way of installing new programs and application in Ubuntu. The preceding word "*sudo*" means *Super User Do*, which is necessary because we need to have higher privileges to install new apps. The word "*git*" is the name of the app we are installing

The second line uses "*cd*" which stands for *Change Directory*, the following word is "*~/*" which is a shortcut for going to the current user's home space.

The third line is using the newly installed app *Git* which *clones* (same thing as download) the ROS source code we need. It comes from our Bitbucket server at *[https://bitbucket.asuqtr.com/scm/subuqtr/ros\_workspace.git](https://bitbucket.asuqtr.com/scm/subuqtr/ros_workspace.git)*

## <u>2\. Build</u><u>ROS and ASUQTR packages</u>

After the download is done, you want to use this line to build ROS and auto install ASUQTR packages. You will be prompted for your OS user password and your Bitbucket account/password.

```
source ~/ros_workspace/scripts/ros_asuqtr_setup
```

The last line will take 10-30 minutes depending on the machine you're installing on and network speed.

<u>*For reference:*</u>

*Ubuntu VM with 1 core made it in 23 minute and an Intel Quantum Processor ran so fast, that it gave me back the 2 hours I spent making this tutorial.*

*WSL 2 with AMD 3900X took 13 minutes.*

Your basic ROS environment should now be setup. To verify if ROS is working properly, type the following line into the terminal:

<sub>*Note : this may not work due to hard to catch errors. If the ros core process does not start, simply open a new terminal and try again.*</sub>

```
roscore
```

This should start the ROS master and show this terminal output :

![](attachments/34897922/34897926.png)

If so, then your ROS installation is working properly. You can stop the ROS master process with Ctrl+C or by closing the terminal

To know if your ASUQTR pacakges are working properly, use your VM's web browser (Firefox),type **localhost** in the address bar and hit Enter. You should see the**ASUQTR dashboard**:

![](attachments/34897922/40697920.png)

You can also reach the **ASUQTR Dashboard**from your physical machine by accessing your **VM's IP address** instead of localhost. To know your VM's address, open a terminal inside it and type the following command:

```
hostname -I
```

You should see something akin to this :![](attachments/34897922/40697921.png)

In case of error, contact [gabriel.tremblay@uqtr.ca](mailto:gabriel.tremblay@uqtr.ca) for troubleshooting or create a ticket at [https://jira.asuqtr.com/servicedesk/customer/portal/1](https://jira.asuqtr.com/servicedesk/customer/portal/1).

## **Conclusion**

**Congratulations!**You've just learned how to use **git** to download your desired repository and installed ROS + ASUQTR packages.

If you need more information as to why and how to use catkin\_tools, visit :

[https://catkin-tools.readthedocs.io/en/latest/](https://catkin-tools.readthedocs.io/en/latest/)

For explanations on how to do common tasks with git, such as switching branch, committing or pushing, check out these :

[https://devconnected.com/how-to-switch-branch-on-git/](https://devconnected.com/how-to-switch-branch-on-git/)

[https://www.git-tower.com/learn/git/commands/git-commit](https://www.git-tower.com/learn/git/commands/git-commit)

[https://www.atlassian.com/git/tutorials/syncing/git-push](https://www.atlassian.com/git/tutorials/syncing/git-push)

As these concepts are not easy to grasp at first, please ask your fellow ASUQTR senior team members for help when in doubt, they'll gladly help.

## Related articles

* Page:

[V.a How to Setup Development Environment for Missions (PC)](/pages/viewpage.action?pageId=36175900)
* Page:

[How-to articles](/display/SUBUQTR/How-to+articles)
* Page:

[How to: Deep Learning](/display/SUBUQTR/How+to%3A+Deep+Learning)
* Page:

[How to: Software](/display/SUBUQTR/How+to%3A+Software)
* Page:

[Running/testing ROS node in Docker](/pages/viewpage.action?pageId=29851650)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-7-23\_23-8-35.png](attachments/34897922/34897923.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-9-15.png](attachments/34897922/34897924.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-10-27.png](attachments/34897922/34897925.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-17-31.png](attachments/34897922/34897926.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-30-28.png](attachments/34897922/34897927.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-30-46.png](attachments/34897922/34897928.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-31-44.png](attachments/34897922/34897929.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-33-52.png](attachments/34897922/34897930.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-7-23\_23-40-49.png](attachments/34897922/34897931.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-13\_18-53-47.png](attachments/34897922/36175877.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-8-14\_16-9-8.png](attachments/34897922/36175897.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-24\_19-58-8.png](attachments/34897922/40697920.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-24\_20-1-30.png](attachments/34897922/40697921.png) (image/png)

## Comments:

||
|---|
|<font>Great article</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Jul 24, 2020 09:31|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)