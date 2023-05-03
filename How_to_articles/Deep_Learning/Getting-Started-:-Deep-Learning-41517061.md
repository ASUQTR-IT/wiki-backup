1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Deep Learning](42827859.html)

# Sous-Marin ASUQTR : Getting Started : Deep Learning

Created by Unknown User (perreaultm), last modified by Maxime Verreault on Oct 28, 2020

If you're here, you probably want to learn more about the deep learning aspect of the autonomous underwater vehicle (AUV).

* [1\. What is deep learning ?](#GettingStarted:DeepLearning-1.Whatisdeeplearning?)
* [2\. Why would we use deep learning for the AUV?](#GettingStarted:DeepLearning-2.WhywouldweusedeeplearningfortheAUV?)
* [3\. How to implement deep learning?](#GettingStarted:DeepLearning-3.Howtoimplementdeeplearning?)

  * [3.1 The dataset](#GettingStarted:DeepLearning-3.1Thedataset)
  * [3.2 The training](#GettingStarted:DeepLearning-3.2Thetraining)
  * [3.3 The implementation](#GettingStarted:DeepLearning-3.3Theimplementation)

# 1\. What is deep learning ?

First of all, it's important to clarify what is deep learning. **Deep learning** (also known as **deep structured learning**) is part of a broader family of **machine learning** methods based on**artificial neural networks**.

**Machine learning** is a kind of **artificial intelligence**. The algorithms use various information like pictures, numbers, words, etc. to learn with patterns.

**Why deep learning and not only machine learning or just artificial neural network?** Simply because deep learning uses more layers than the previous methods which makes it more specialized. We often compare deep learning to a human brain due to its complexity.Here is a summary of that:

![](https://datawider.com/wp-content/uploads/2019/11/Machine-Learning-vs-Deep-Learning.png)

# 2\. Why would we use deep learning for the AUV?

The reason why we use deep learning for AUV is that it is very powerful to use in object detection and image classification. These features help the mission manager to identify which type of mission we have and where is the AUV.Here is an example of object detection we made for the AUV in action:  
  
However, it requires a lot of data to be efficient but is still more efficient than machine learning.

For more information about deep learning in general, Mathworks made a great article about it: [https://www.mathworks.com/discovery/deep-learning.html](https://www.mathworks.com/discovery/deep-learning.html)

# 3\. How to implement deep learning?

There are many ways to implement a deep learning algorithm but three steps are always necessary no matter which way you choose: **dataset, training and implementation**.

## 3.1 The dataset

```
This step is essential since it is the one that is decisive for the other two. It consists of preparing the data so that it can be used for learning. For example, for object detection the object in question must be identified (labelled). 
```

Many tools exist for labelling. The ASUQTR team chose *Labelbox* for this task because it allows many members to label at the same time. This is saving us so much time considering the amount of data to be processed for deep learning.

![](https://files.helpdocs.io/3fu9z5w5lj/logo.png?t=1566725235309)

## 3.2 The training

```
During this step, the dataset from the previous step goes through a neural network model in order to determine the discriminating characters. You can create your own model but be aware that doing this often requires a lot of training to achieve accuracy. Usually, pre-trained models are used like Alexnet or Googlenet. Training a model requires a lot of GPU memory so the more you have, the faster it will be.
```

**![](https://docs.nvidia.com/deeplearning/digits/digits-user-guide/graphics/home-page-digits.png)![](https://images.exxactcorp.com/CMS/landing-page/resource-center/supported-software/logo/Deep-Learning/DIGITS.png)**

For this step, the ASUQTR team chose an NVIDIA tool named *Digits*.We chose this tool because it can handle multiple NVIDIA graphic cards and it is open-source.You can also easily visualize the accuracy of your model with this tool.

## 3.3 The implementation

The last step is to implement the trained model into the system in order to use it.Although this step seems simple, it is relatively complicatedso the ASUQTR team chose to use Jetson Inference. It is a set of libraries made by NVIDIA developers to deploy neural networks.  
For more information about Jetson Inference, see [https://github.com/dusty-nv/jetson-inference](https://github.com/dusty-nv/jetson-inference)  
  

* Page:

[How to create a dataset](/display/SUBUQTR/How+to+create+a+dataset)(Sous-Marin ASUQTR)

  * [kb-how-to-article](/label/SUBUQTR/kb-how-to-article)
  * [deeplearning](/label/SUBUQTR/deeplearning)
* Page:

[Getting Started : Deep Learning](/display/SUBUQTR/Getting+Started+%3A+Deep+Learning)(Sous-Marin ASUQTR)

  * [kb-how-to-article](/label/SUBUQTR/kb-how-to-article)
  * [deeplearning](/label/SUBUQTR/deeplearning)
  * [machine\_learning](/label/SUBUQTR/machine_learning)
* Page:

[How to train a model with NVIDIA Digits](/display/SUBUQTR/How+to+train+a+model+with+NVIDIA+Digits)(Sous-Marin ASUQTR)

  * [nvidia](/label/SUBUQTR/nvidia)
  * [deeplearning](/label/SUBUQTR/deeplearning)
  * [kb-how-to-article](/label/SUBUQTR/kb-how-to-article)
* Page:

[How to use a custom trained model in Jetson-Inference](/display/SUBUQTR/How+to+use+a+custom+trained+model+in+Jetson-Inference)(Sous-Marin ASUQTR)

  * [nvidia](/label/SUBUQTR/nvidia)
  * [deeplearning](/label/SUBUQTR/deeplearning)
  * [kb-how-to-article](/label/SUBUQTR/kb-how-to-article)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-10-27\_20-37-1.png](attachments/41517061/41517090.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)