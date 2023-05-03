1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Deep Learning](42827859.html)

# Sous-Marin ASUQTR : How to create a dataset

Created by Unknown User (perreaultm), last modified on Jan 20, 2021

*When you want to learn how to make a dataset usable for the training tool chosen by the ASUQTR team, NVIDIA Digits*

** [Requirements](#Howtocreateadataset-Requirements)
* [1\. What's NVIDIA Digits ?](#Howtocreateadataset-1.What'sNVIDIADigits?)
* [2\. Dataset and KITTI format](#Howtocreateadataset-2.DatasetandKITTIformat)
* [3\. Labelbox](#Howtocreateadataset-3.Labelbox)

  * [3.1 Import the images](#Howtocreateadataset-3.1Importtheimages)
  * [3.2 Setting up your project](#Howtocreateadataset-3.2Settingupyourproject)
  * [3.3 Labelling your images](#Howtocreateadataset-3.3Labellingyourimages)
  * [3.4 Export the labels](#Howtocreateadataset-3.4Exportthelabels)
* [4\. Transform JSON format to KITTI format](#Howtocreateadataset-4.TransformJSONformattoKITTIformat)*

# Requirements

* A full set of images of the object(s) you want to train (for example : Gate, pole, etc.)

# 1\. What's NVIDIA Digits ?

```
NVIDIA Digits is a tool designed to perform deep learning tasks. The advantage of this tool is that it is interactive which allows you to spend more time in model design rather than programming and debugging. You can also visualize each layer of your neural network and directly test your model.
```

We will see in a future article how to use this tool. For now, the most important information to remember for the dataset is that the tool **only works** with the **KITTI format.**

# 2\. Dataset and KITTI format

A **dataset** is composed of **images** and **label(s)** files that defines the object(s) identified in the corresponding image.Keep in mind **that the more images a dataset contains, the longer it will take to train the dataset** **but the precision will be just as high.**

KITTI is simply a format made by the Karlsruhe Institute of Technology Toyota Technological Institute (KITTI). It is simply a text file where each information of the label is separated by a space. Here is an example :

```
Car -1 -1 0 0 0 0 0 0 1.7479205131530762 4.543853282928467 -4.850294888019562 0 12.025148266553884 2.5170462131500244 0.8644813299179077
Car -1 -1 0 0 0 0 0 0 1.6591367721557617 3.532350778579712 1.8141979277133942 0 60.5813756108284 -1.5696606636047363 0.7821810841560364
Car -1 -1 0 0 0 0 0 0 1.5887395143508911 3.827711343765259 2.2965295612812042 0 15.157683968544013 -1.6072721481323242 0.7636483311653137
Car -1 -1 0 0 0 0 0 0 1.692084550857544 4.111057758331299 6.048987460136409 0 21.396564126014717 -0.7621548771858215 0.5922860503196716
Car -1 -1 0 0 0 0 0 0 1.6605327129364014 4.091874122619629 2.171874260902399 0 37.970115923881536 -1.504165768623352 0.5084235072135925
```

Here is the definition of each term on a line :

![](https://images3.programmersought.com/688/f3/f34593087869cf50ab725ae6d76a1570.png)

Dontcare


"Dontcare" is a special class! We use it to specify an object that we want to identify but which is not complete in the image or which could skew the results. Use it carefully!

# 3\. Labelbox

ASUQTR team uses Labelbox to create labels on the image in the dataset. Let's see the steps to follow to make the labels correctly.

To log to Labelbox, simply go to the [Labelbox website](https://labelbox.com/) and use your account to log in. Contact the team captain if you don't have an account.

## 3.1 Import the images

On your main page, simply select the **Datasets** section and click on **Add dataset.**

**![](attachments/40697978/41517113.png)**

Simply drag your images to add it to Labelbox.

![](attachments/40697978/41517114.png)

## 3.2 Setting up your project

First, let's start a new project on Labelbox. Select the **Projects** section on the main page and click on **New** **project.**

![](attachments/40697978/41517117.png)

Enter your **project name**.

![](attachments/40697978/41517118.png)

In general, we use the project name to describe what does the dataset talk about.

Next, you need to select your dataset you previously added to Labelbox. To select it, click on **Attach.**

![](attachments/40697978/41517119.png)

The final step for setting up your project is to configure the label editor. In other words, this step is to set which objects you want to identify (classify) and how you want to make your label for it. Click on **Edit**to set up everything.

![](attachments/40697978/41517121.png)

Here you have a preview of what your labelling window will look like. Just add your **objects to detect**and how to make the label. For our application, simply select **Bounding box.**

**![](attachments/40697978/41517122.png)**

Classifications are **not important for our application** so don't pay attention to it. Just **Confirm** when all your desired objects are here and then click on **Complete setup.**

## 3.3 Labelling your images

This task is the longest but not the most difficult. Click on **Start labelling**on your project window.You can also add members to help you complete labelling faster. Simply, click on **Add member.**

**![](attachments/40697978/41517123.png)**

And that's where the real job starts. You have to go into the **entire** dataset of images to make labels on each. For example, it takes me 6.6 hours to pass an entire dataset of 9500 images. Simply **choose the right label** for the object in the image or **skip** if no object to label is in the image. Labelbox will not create a label when an image is skipped.

Shortcuts


Labelbox contains shortcuts (thank god) to help you spend less time on your mouse. Simply click here for more information about them :

![](attachments/40697978/41517124.png)

Keep in mind that if an object is **too difficult to identify for you** or **too big or too small** in the image, it is **better to skip** the image rather than label it. Here is an example of a too-small object :

![](attachments/40697978/41517125.png)

In this photo, the pole isway too small to have any relevant information.

Maximum/minimum box size


*Digits* has a minimum (50x50 pixels) and maximum (400x400 pixels) value for the label boxes ! Be sure to respect them to avoid bad training results.

## 3.4 Export the labels

When everything is done, just select the **Export** section on your project page. Choose the **JSON format**and click on **Generate export**. That's pretty much it for the Labelbox part.

**![](attachments/40697978/41517126.png)**

# 4\. Transform JSON format to KITTI format

Labelbox can export your labels but not at the KITTI format intended. To do the conversion, the ASUQTR team did a python program to easily make the conversion.

Here is the code : [labelboxtoKitti.py](https://drive.google.com/drive/u/0/folders/1-hQpqYVsi4EZ28Jvm43pRrJAnfv9wZUe)

labelboxtoKitti.py

```
#!/usr/bin/python3
#-*- coding: utf-8 -*-

#Auteur: Marc-André Morrissette-Soucy
import json
import os
import requests
from PIL import Image

path = '/home/asuqtr/Documents/11-14-2020/'
file = 'export-2020-10-01T00 18 28.731Z.json'
#Load labelbox Json
with open(path+file) as f_jason:
    lblBox = json.load(f_jason)
s = "."

#New directory creation
fl = file.split('.')
m_path = path +s.join(fl[:-1])
os.mkdir(m_path)
img_path = m_path+'/image'
os.mkdir(img_path)
lbl_path = m_path+'/label'
os.mkdir(lbl_path)

#request new size from user
print("How big are the output images? width,height")
W,H = input().split(',')
img_width = int(W)
img_height = int(H)

#file and label name
file_name = "IMG"
i = 1
j = 1

nb_line = len(lblBox)

for u in lblBox:
    if u.get('Label').get('objects'):
        name_img =img_path+"/"+file_name + str(i)
        name_lbl =lbl_path+"/"+file_name + str(i)

        #image Download from labelbox
        url = u.get('Labeled Data')
        r = requests.get(url, allow_redirects=True)
        open(name_img+".jpeg", 'wb').write(r.content)
        
        #Image resize
        image = Image.open(name_img+".jpeg")
        old_width, old_height = image.size
        new_image = image.resize((img_width, img_height))
        new_image.save(name_img+".jpeg")

        f_txt = open(name_lbl+".txt", "w")
        for o in u.get('Label').get('objects'):
            top= o.get('bbox').get('top')
            left= o.get('bbox').get('left')
            height= o.get('bbox').get('height')
            width= o.get('bbox').get('width')
            obj = o.get('title')
            obj = obj.split("'")[0]
            #bbox resize 
            x1 = int((left/old_width)*img_width)
            y1 = int((top/old_height)*img_height)
            x2 = int(((left+width)/old_width)*img_width)
            y2 = int(((top+height)/old_height)*img_height)
            ss = ' '
            f_txt.writelines(obj + ' 0.00 0 0.00 ' + str(x1) + '.00 ' + str(y1) + '.00 ' + str(x2) + '.00 ' + str(y2) + '.00 0.00 0.00 0.00 0.00 0.00 0.00 0.00\n')
        f_txt.close()
        i = i + 1
    j = j + 1
    print(str(j)+'/'+str(nb_line))

print(str(i) + ' good object process') 
```

This code make four tasks :

* Renames all files (labels and images) to be sure each image has the right label.
* Deletes all images skipped during the Labelbox step.
* Resize your images at the desired format (ex 640x640) and adapt the label with it.
* Converts the JSON format into the KITTI one.
* Splits images and labels into folders for training and testing (70% for training and 30% for testing).

And that's pretty much it. Now, your dataset is perfect and ready to be used by Digits to make a great object detection model !

![](https://media.tenor.com/images/73cca45a93f91944b2c9fdd4b05c3c53/tenor.gif)

* Page:

[How to use a custom trained model in Jetson-Inference](/display/SUBUQTR/How+to+use+a+custom+trained+model+in+Jetson-Inference)
* Page:

[How to train a model with NVIDIA Digits](/display/SUBUQTR/How+to+train+a+model+with+NVIDIA+Digits)
* Page:

[How to create a dataset](/display/SUBUQTR/How+to+create+a+dataset)
* Page:

[Getting Started : Deep Learning](/display/SUBUQTR/Getting+Started+%3A+Deep+Learning)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-10-28\_12-50-25.png](attachments/40697978/41517108.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_12-51-8.png](attachments/40697978/41517109.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-8-37.png](attachments/40697978/41517112.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-10-5.png](attachments/40697978/41517113.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-11-33.png](attachments/40697978/41517114.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-14-40.png](attachments/40697978/41517117.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-16-12.png](attachments/40697978/41517118.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-20-7.png](attachments/40697978/41517119.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-29-26.png](attachments/40697978/41517121.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-32-10.png](attachments/40697978/41517122.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-44-39.png](attachments/40697978/41517123.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-52-31.png](attachments/40697978/41517124.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_14-57-44.png](attachments/40697978/41517125.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-10-28\_15-2-47.png](attachments/40697978/41517126.png) (image/png)

## Comments:

||
|---|
|<font>L'exemple du format de KITTI devrait être *copy-pastable*. (Code block)</font>![](images/icons/contenttypes/comment_16.png) Posted by niggamax24 at Oct 28, 2020 16:08|
|<font>C'est fait !</font>![](images/icons/contenttypes/comment_16.png) Posted by Perreaultm at Oct 29, 2020 13:01|

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)