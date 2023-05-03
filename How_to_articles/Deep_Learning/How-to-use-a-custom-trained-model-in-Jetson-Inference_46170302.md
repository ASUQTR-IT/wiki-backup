1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Deep Learning](42827859.html)

# Sous-Marin ASUQTR : How to use a custom trained model in Jetson-Inference

Created by Unknown User (perreaultm) on Mar 16, 2021

*When you want to learn how to use a custom trained model in Jetson Inference.*

*­­­*

* [Requirements](#HowtouseacustomtrainedmodelinJetsonInference-Requirements)
* [1\. What's Jetson-Inference ?](#HowtouseacustomtrainedmodelinJetsonInference-1.What'sJetson-Inference?)
* [2\. Files to use for Inference](#HowtouseacustomtrainedmodelinJetsonInference-2.FilestouseforInference)
* [3\. Test your trained model](#HowtouseacustomtrainedmodelinJetsonInference-3.Testyourtrainedmodel)

  * [3.1 Launch on ASUQTR ROS](#HowtouseacustomtrainedmodelinJetsonInference-3.1LaunchonASUQTRROS)
  * [3.1 Launch on Jetson Inference node directly](#HowtouseacustomtrainedmodelinJetsonInference-3.1LaunchonJetsonInferencenodedirectly)

# Requirements

* [Custom trained model](How-to-train-a-model-with-NVIDIA-Digits_42139672.html)

* Jetson-inference installed
* Camera (webcam can work) connected.

Jetson-Inference


To install Jetson-Inference, just install [the ROS and ASUQTR packages](IV.-How-to-install-ROS-and-ASUQTR-packages_34897922.html). Be sure to know how to start and use ROS.

# 1\. What's Jetson-Inference ?

![](https://github.com/dusty-nv/jetson-inference/raw/master/docs/images/deep-vision-header.jpg)

Jetson-Inference is a library made by NVIDIA developers especially designed for deploying neural networks like object detection or image recoginition with the NVIDIA architecture called TensorRT.

![](attachments/46170302/46170303.png)

I'm not gonna go deep in the details because it's complex and it's not the point of this tutorial but if you want to know the deep details of how it works, here's links to help you find more informations :

Jetson-Inference : [https://github.com/dusty-nv/jetson-inference](https://github.com/dusty-nv/jetson-inference)

TensorRT : [https://developer.nvidia.com/tensorrt](https://developer.nvidia.com/tensorrt)

The major advantage of these librairies is they contain deep learning nodes structure for ROS which means they can fit for our ASUQTR architecture with no big modifications to made and they can easily publish computer vision informations for our mission manager.

# 2\. Files to use for Inference

To test your model, you will need the zip file created by NVIDIA Digits before.

![](attachments/46170302/46170304.png)

In the zip file, only two files are important : the **deploy.prototxt** who contains the frame of the neural networks and the **.caffemodel** who contains your trained model. Be sure to put these two files in your ubuntu machine who runs Jetson-Inference.

You will need .txt file made by you who contains the classes of your model. Put it with the two other files. Be sure to respect **the order of your classes**. Here's an exemple :

```
Gate
Poteau
```

Here, Gate is class number one and Poteau is class number two.

Be sure these files are **installed**in your device (VM, Jetson device, etc.)

# 3\. Test your trained model

To help you understand how to test your model, let's see parameters to put for your model into the ROS node.

![](attachments/46170302/46170323.png)

The **model\_name** parameter function is pretty obvious. Specify the name of your model train on Digits. You can also use name of already built models installed on Jetson Inference to validate Jetson Inference works fine.

The **model\_path and prototxt\_path**parameter specify the path for your model. In our case, it's the path to the .**caffemodel and the .prototxt** files produced by Digits.

The **input\_blob, output\_cvg and output\_bbox** parameters specify the name of specific layers of your Deep Neural Network (DNN). Usually, we use the default one and it's our case so keep them like that.

The **class\_label\_path**parameter specify the file for your classes. It's the path for the **.txt file**you create before.

**Class\_labels\_HASH, overlay\_flags and mean\_pixel\_value**are parameters not important in our case so keep them default.

The **threshold**parameter specify at which precision the bbox of an object appears in the screen. **0.8** is a good value in many case.

The launch of your model on ROS will depend of your device you launch the node.

## 3.1 Launch on ASUQTR ROS

For my tests, I used a Jetson AGX Xavier. Here's the command I used in a terminal :

```
roslaunch asuqtr_mission_management jetson.launch model_path:="/home/asuqtr/file_model/snapshot_iter_14812.caffemodel" prototxt_path:="/home/asuqtr/file_model/deploy_1.prototxt" class_labels_path:="/home/asuqtr/file_model/labels.txt" input_blob:="data" output_cvg:="coverage" output_bbox:="bboxes" model_name:="test2_2" threshold:=0.8
```

Just change the different parameters according to your situation (ex paths) and be sure **the camera is connected on your device.**

If everything works fine, the ros\_deep\_learning node publish informations from the camera in the node. To validate everything works perfectly, go to the ASUQTR control web page. You can also use the following adress on your web browser to see the results :

```
http://"IP of the device":8080/stream_viewer?topic?=/detectnet/overlay
```

Here's an example of what it's supposed to be :

## 3.1 Launch on Jetson Inference node directly

Maybe for your application you don't need to launch the whole ASUQTR ROS system. Here's how to launch only the node ros\_deep\_learning.

```
roslaunch ros_deep_learning detecnet.ros1.launch input:=file:///home/perreaultm/GOPR0008.MP4 output:=file:///home/perreaultm/result.AVI model_path:="/home/asuqtr/file_model/snapshot_iter_14812.caffemodel" prototxt_path:="/home/asuqtr/file_model/deploy_1.prototxt" class_labels_path:="/home/asuqtr/file_model/labels.txt" input_blob:="data" output_cvg:="coverage" output_bbox:="bboxes" model_name:="test2_2" threshold:=0.8
```

The parameters are almost the same as before. You just need to specify the **input** and **output.**Here's valid values for each :

![](attachments/46170302/46170330.png)

Default values


By default, it use a camera for input and a window in your screen (display) for output.

As you can see, many different options like a video, a camera or also simply an image are possible. Here's an example I made with a video in input and output.

That's it for all the part of deep learning for ASUQTR. Feel free to contact me if errors or incomprehension occur during your tests. I will do my best to help you ![(big grin)](images/icons/emoticons/biggrin.svg)

* Page:

[The wordpress website](/display/SUBUQTR/The+wordpress+website)
* Page:

[How to work remotely using Jetbrains IDE](/display/SUBUQTR/How+to+work+remotely+using+Jetbrains+IDE)
* Page:

[Agile Guidelines](/display/SUBUQTR/Agile+Guidelines)
* Page:

[How to: borrow an item](/display/SUBUQTR/How+to%3A+borrow+an+item)
* Page:

[How to make a pull request for FlexBE Behaviors](/display/SUBUQTR/How+to+make+a+pull+request+for+FlexBE+Behaviors)

## Attachments:

![](images/icons/bullet_blue.gif) [image2021-2-26\_11-31-21.png](attachments/46170302/46170303.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-2-26\_11-59-16.png](attachments/46170302/46170304.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2021-3-10\_11-50-21.png](attachments/46170302/46170323.png) (image/png)  
![](images/icons/bullet_blue.gif) [Screenshot from 2021-03-16 19-19-25.png](attachments/46170302/46170330.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)