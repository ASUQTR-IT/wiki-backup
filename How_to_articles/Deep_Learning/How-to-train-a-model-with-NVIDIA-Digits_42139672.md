1. [Sous-Marin ASUQTR](index.html)
2. [Sous-Marin ASUQTR Knowledge Center](Sous-Marin-ASUQTR-Knowledge-Center_5144578.html)
3. [How-to articles](How-to-articles_13533186.html)
4. [How to: Deep Learning](42827859.html)

# Sous-Marin ASUQTR : How to train a model with NVIDIA Digits

Created by Unknown User (perreaultm), last modified on Jan 29, 2021

*When you want to learn how to make a trained model with NVIDIA Digits usable by Jetson Inference.*

** [Requirements](#HowtotrainamodelwithNVIDIADigits-Requirements)
* [1\. Preparation](#HowtotrainamodelwithNVIDIADigits-1.Preparation)
* [2\. Make a trained model](#HowtotrainamodelwithNVIDIADigits-2.Makeatrainedmodel)

  * [2.1 Import the dataset](#HowtotrainamodelwithNVIDIADigits-2.1Importthedataset)
  * [2.2 Set up the training](#HowtotrainamodelwithNVIDIADigits-2.2Setupthetraining)

    * [2.2.1 Pretrained models](#HowtotrainamodelwithNVIDIADigits-2.2.1Pretrainedmodels)
  * [2.3 Training phase](#HowtotrainamodelwithNVIDIADigits-2.3Trainingphase)

    * [2.3.1 Retraining](#HowtotrainamodelwithNVIDIADigits-2.3.1Retraining)
  * [2.4 Export your model](#HowtotrainamodelwithNVIDIADigits-2.4Exportyourmodel)*

# Requirements

* [A dataset in KITTI format](How-to-create-a-dataset_40697978.html)

* A Ubuntu 18.04 PC with 12 Go GPU RAM and NVIDIA *Digits*installed on it

GPU RAM


You can run *Digits* with less RAM than 16 Go but it will take more time to complete your training (in hours). *Digits* can handle multiple graphic cards.

# 1\. Preparation

Before starting to train your model, be sure **your dataset made previously is saved on the PC with *Digits* .**Note **the path of your files** for the future.

To log to *Digits,* use the IP adress with the port set for *Digits* in a web browser. You will see this home page if it's working :

![](attachments/42139672/42139674.png)

# 2\. Make a trained model

Now everything is set in *Digits,* let's make our first trained model.

## 2.1 Import the dataset

To import your dataset into *Digits,*select the **Datasets section** and click on the **New Dataset button.**Select **O** **bject Detection**.

![](attachments/42139672/42139675.png)

Now, let's take a look of all options to set up in the next page :

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/123727984_859517984585552_993350443882668747_n.png?_nc_cat=110&ccb=2&_nc_sid=ae9488&_nc_ohc=j5RDYgmOxg8AX-pdsFR&_nc_ht=scontent.fymy1-2.fna&oh=a84d2d2678fc0703c1d6041d53230a48&oe=603AA76F)

Here is where you specify each folders of your dataset. Luckily for you, the python code used during the dataset section made those folders. Simply**specify the path for each of them.**

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/123843293_361543474907606_1784019809762334607_n.png?_nc_cat=100&ccb=2&_nc_sid=ae9488&_nc_ohc=yHuRcb8J1AcAX-PPzfb&_nc_ht=scontent.fymy1-2.fna&oh=0b60e2b95b8b3ac96d6c565a2916112b&oe=6037D84A)

With **Pad image** and **Resize image** option, you can **change the size of all of your images** in your dataset. We use this option to force an image size to **match with a pretrained model image size**. In this example, the pretrained model only accepts 720x480 images so we need to resize them.

You can set a **minimum box size for the validation set**to only retain boxes higher than the specify value. Setting this to 0 disable this option.

The **Custom classes** option is where you specify the name of your classes. Separates them with a comma.

Dontcare class


Dontcare class <u>**MUST BE**</u> the first class to specify (used or not) because the pretrained model use for this tutorial ignores the first class (the first class is always Dontcare).

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/123858040_987108165120385_7105763129693872261_n.png?_nc_cat=105&ccb=2&_nc_sid=ae9488&_nc_ohc=oIU5Jbsdi-MAX8PvZk9&_nc_ht=scontent.fymy1-2.fna&oh=94f4877687cd3882d21da2b0a04058f3&oe=603B8BC5)

Here, the two options really important are **Group Name** and **Dataset Name**. It's simply the group where *Digits* stocks the dataset with his name.

Click on **Create**when all important options are set up. It will take around 5-10 mins depending how much GPU RAM you have.

## 2.2 Set up the training

Now it's time to launch your training. On the Home page, select the **Models section**and click on **New Model** button. Select **Object detection**.

![](attachments/42139672/42139687.png)

Let's take a look at the training options.

![Aucune description disponible.](https://scontent-lga3-2.xx.fbcdn.net/v/t1.15752-9/123940345_672163800335472_2181398835203241960_n.png?_nc_cat=108&ccb=2&_nc_sid=ae9488&_nc_ohc=Pt7n_hQ-UhcAX_qaNFp&_nc_ht=scontent-lga3-2.xx&oh=b63df1f9f7ceaee51c42bff04ae373fa&oe=6037DD79)

First, you need to **select your dataset** on the **Select Dataset** option.

Select the numbers of epochs you want for training your model in the **Training epochs** option. Keep in mind that a**model with a huge amount of epochs will be longer to train**. 100 epochs is a good number to start with or less to be sure your training is working fine.

Epoch


**Epoch** is the number of times (not necessarily iterations) your dataset will pass throught the entire model. For example, for a dataset of 7000 images, a model with 100 epochs will pass the 7000 images throught the model 100 times.

**Substract mean** option must be to none.

![Aucune description disponible.](https://scontent-lga3-2.xx.fbcdn.net/v/t1.15752-9/123937200_723102194961891_7151431539563760933_n.png?_nc_cat=108&ccb=2&_nc_sid=ae9488&_nc_ohc=XmTSvAyF9OYAX9p-axo&_nc_ht=scontent-lga3-2.xx&oh=cef4fc4eb34dd0989080c8e1243d4df3&oe=603A1480)

The **Batch size** option set the batch size you want to use. **Leaving this field blank will take the network define batch size**. In our model, Batch size is set to 10.

The **Batch accumulation** option is useful when you can't use the real batch size value define by your network because of your GPU RAM. For example, if you can't use batch size of 10, try a batch size of 5 and a batch accumulation of 2 (5\*2=10). It's helping to fit your Batch size into your memory. Leaving this blank will desactivate this option (set it to 1).

Batch size


Batch size is the number of samples who will pass throught the model at the same time. For example, with a batch size of 10, 10 images will pass throught the model at the same time at each iteration.

Select the solver you want with the **Solver type** option. In most situations, **Adam** is the best solver because **it's converge faster than the others solvers**.

Choose a learning rate with the **Base Learning Rate** option. With 100 epochs, 0.0001 seems the best value.

Learning rate


Learning rate is the speed your model "learns" It's sometimes referred as a **gain**. Setting this too high will provide bad results because it's too fast and to low will take too much time to converge to good results.

Click on **Show advanced learning rate** **options**box and select the Exponential Decay on the **Policy** option. On **Gamma** option, enter 0.95 for better results. You can see what your learning rate curve will look like with the **Visualize LR** button.

### 2.2.1 Pretrained models

*Digits*also has a feature to let you use a petrained model for your training. Basically, this improve your training time because some features of your model will be set in the model architecture. *Digits*doesn't have a big library but one of them is the one we will take : [Detectnet](https://developer.nvidia.com/blog/detectnet-deep-neural-network-object-detection-digits/). It's a derivated model from Googlenet.

Here is the architecture of Googlenet :

![](https://cdn.analyticsvidhya.com/wp-content/uploads/2018/10/googlenet.png)

Has you can see, Googlenet has many layers (22 layers exactly). That's make him a more complex model (also mean more RAM needed) than the others but more precise too. Don't worry ! You don't have to understand each of these layers (even me doesn't know each of them).

Now let's get back to our topics.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/124038325_395804348117056_5991411200770758203_n.png?_nc_cat=105&ccb=2&_nc_sid=ae9488&_nc_ohc=9B9jVyRD96sAX-VisZa&_nc_ht=scontent.fymy1-2.fna&oh=a4c74a788851708f4c2a88c5949fd91f&oe=603B537E)

On the **Custom Network** section, you will have to put the **Detecnet model**. Model are prototxt files. Here is the Detecnet one :

Detectnet.prototxt

You can download it [here](https://github.com/dusty-nv/jetson-inference/blob/master/data/networks/detectnet.prototxt).

```
# DetectNet network

# Data/Input layers
name: "DetectNet"
layer {
  name: "train_data"
  type: "Data"
  top: "data"
  data_param {
    backend: LMDB
    source: "examples/kitti/kitti_train_images.lmdb"
    batch_size: 10
  }
  include: { phase: TRAIN }
}
layer {
  name: "train_label"
  type: "Data"
  top: "label"
  data_param {
    backend: LMDB
    source: "examples/kitti/kitti_train_labels.lmdb"
    batch_size: 10
  }
  include: { phase: TRAIN }
}
layer {
  name: "val_data"
  type: "Data"
  top: "data"
  data_param {
    backend: LMDB
    source: "examples/kitti/kitti_test_images.lmdb"
    batch_size: 6
  }
  include: { phase: TEST stage: "val" }
}
layer {
  name: "val_label"
  type: "Data"
  top: "label"
  data_param {
    backend: LMDB
    source: "examples/kitti/kitti_test_labels.lmdb"
    batch_size: 6
  }
  include: { phase: TEST stage: "val" }
}
layer {
  name: "deploy_data"
  type: "Input"
  top: "data"
  input_param {
    shape {
      dim: 1
      dim: 3
      dim: 640
      dim: 640
    }
  }
  include: { phase: TEST not_stage: "val" }
}

# Data transformation layers
layer {
  name: "train_transform"
  type: "DetectNetTransformation"
  bottom: "data"
  bottom: "label"
  top: "transformed_data"
  top: "transformed_label"
  detectnet_groundtruth_param: {
    stride: 16
    scale_cvg: 0.4
    gridbox_type: GRIDBOX_MIN
    coverage_type: RECTANGULAR
    min_cvg_len: 20
    obj_norm: true
    image_size_x: 640
    image_size_y: 640
    crop_bboxes: false
    object_class: { src: 1 dst: 0} # obj class 1 -> cvg index 0
  }
   detectnet_augmentation_param: {
    crop_prob: 1
    shift_x: 32
    shift_y: 32
    flip_prob: 0.5
    rotation_prob: 0
    max_rotate_degree: 5
    scale_prob: 0.4
    scale_min: 0.8
    scale_max: 1.2
    hue_rotation_prob: 0.8
    hue_rotation: 30
    desaturation_prob: 0.8
    desaturation_max: 0.8
  }
  transform_param: {
    mean_value: 127
  }
  include: { phase: TRAIN }
}
layer {
  name: "val_transform"
  type: "DetectNetTransformation"
  bottom: "data"
  bottom: "label"
  top: "transformed_data"
  top: "transformed_label"
  detectnet_groundtruth_param: {
    stride: 16
    scale_cvg: 0.4
    gridbox_type: GRIDBOX_MIN
    coverage_type: RECTANGULAR
    min_cvg_len: 20
    obj_norm: true
    image_size_x: 640
    image_size_y: 640
    crop_bboxes: false
    object_class: { src: 1 dst: 0} # obj class 1 -> cvg index 0
  }
  transform_param: {
    mean_value: 127
  }
  include: { phase: TEST stage: "val" }
}
layer {
  name: "deploy_transform"
  type: "Power"
  bottom: "data"
  top: "transformed_data"
  power_param {
    shift: -127
  }
  include: { phase: TEST not_stage: "val" }
}

# Label conversion layers
layer {
  name: "slice-label"
  type: "Slice"
  bottom: "transformed_label"
  top: "foreground-label"
  top: "bbox-label"
  top: "size-label"
  top: "obj-label"
  top: "coverage-label"
  slice_param {
    slice_dim: 1
    slice_point: 1
    slice_point: 5
    slice_point: 7
    slice_point: 8
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "coverage-block"
  type: "Concat"
  bottom: "foreground-label"
  bottom: "foreground-label"
  bottom: "foreground-label"
  bottom: "foreground-label"
  top: "coverage-block"
  concat_param {
    concat_dim: 1
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "size-block"
  type: "Concat"
  bottom: "size-label"
  bottom: "size-label"
  top: "size-block"
  concat_param {
    concat_dim: 1
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "obj-block"
  type: "Concat"
  bottom: "obj-label"
  bottom: "obj-label"
  bottom: "obj-label"
  bottom: "obj-label"
  top: "obj-block"
  concat_param {
    concat_dim: 1
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "bb-label-norm"
  type: "Eltwise"
  bottom: "bbox-label"
  bottom: "size-block"
  top: "bbox-label-norm"
  eltwise_param {
    operation: PROD
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "bb-obj-norm"
  type: "Eltwise"
  bottom: "bbox-label-norm"
  bottom: "obj-block"
  top: "bbox-obj-label-norm"
  eltwise_param {
    operation: PROD
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}

######################################################################
# Start of convolutional network
######################################################################

layer {
  name: "conv1/7x7_s2"
  type: "Convolution"
  bottom: "transformed_data"
  top: "conv1/7x7_s2"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    pad: 3
    kernel_size: 7
    stride: 2
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "conv1/relu_7x7"
  type: "ReLU"
  bottom: "conv1/7x7_s2"
  top: "conv1/7x7_s2"
}

layer {
  name: "pool1/3x3_s2"
  type: "Pooling"
  bottom: "conv1/7x7_s2"
  top: "pool1/3x3_s2"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 2
  }
}

layer {
  name: "pool1/norm1"
  type: "LRN"
  bottom: "pool1/3x3_s2"
  top: "pool1/norm1"
  lrn_param {
    local_size: 5
    alpha: 0.0001
    beta: 0.75
  }
}

layer {
  name: "conv2/3x3_reduce"
  type: "Convolution"
  bottom: "pool1/norm1"
  top: "conv2/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "conv2/relu_3x3_reduce"
  type: "ReLU"
  bottom: "conv2/3x3_reduce"
  top: "conv2/3x3_reduce"
}

layer {
  name: "conv2/3x3"
  type: "Convolution"
  bottom: "conv2/3x3_reduce"
  top: "conv2/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 192
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "conv2/relu_3x3"
  type: "ReLU"
  bottom: "conv2/3x3"
  top: "conv2/3x3"
}

layer {
  name: "conv2/norm2"
  type: "LRN"
  bottom: "conv2/3x3"
  top: "conv2/norm2"
  lrn_param {
    local_size: 5
    alpha: 0.0001
    beta: 0.75
  }
}

layer {
  name: "pool2/3x3_s2"
  type: "Pooling"
  bottom: "conv2/norm2"
  top: "pool2/3x3_s2"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 2
  }
}

layer {
  name: "inception_3a/1x1"
  type: "Convolution"
  bottom: "pool2/3x3_s2"
  top: "inception_3a/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_3a/relu_1x1"
  type: "ReLU"
  bottom: "inception_3a/1x1"
  top: "inception_3a/1x1"
}

layer {
  name: "inception_3a/3x3_reduce"
  type: "Convolution"
  bottom: "pool2/3x3_s2"
  top: "inception_3a/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 96
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_3a/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_3a/3x3_reduce"
  top: "inception_3a/3x3_reduce"
}

layer {
  name: "inception_3a/3x3"
  type: "Convolution"
  bottom: "inception_3a/3x3_reduce"
  top: "inception_3a/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_3a/relu_3x3"
  type: "ReLU"
  bottom: "inception_3a/3x3"
  top: "inception_3a/3x3"
}

layer {
  name: "inception_3a/5x5_reduce"
  type: "Convolution"
  bottom: "pool2/3x3_s2"
  top: "inception_3a/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 16
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3a/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_3a/5x5_reduce"
  top: "inception_3a/5x5_reduce"
}
layer {
  name: "inception_3a/5x5"
  type: "Convolution"
  bottom: "inception_3a/5x5_reduce"
  top: "inception_3a/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3a/relu_5x5"
  type: "ReLU"
  bottom: "inception_3a/5x5"
  top: "inception_3a/5x5"
}

layer {
  name: "inception_3a/pool"
  type: "Pooling"
  bottom: "pool2/3x3_s2"
  top: "inception_3a/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}

layer {
  name: "inception_3a/pool_proj"
  type: "Convolution"
  bottom: "inception_3a/pool"
  top: "inception_3a/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3a/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_3a/pool_proj"
  top: "inception_3a/pool_proj"
}

layer {
  name: "inception_3a/output"
  type: "Concat"
  bottom: "inception_3a/1x1"
  bottom: "inception_3a/3x3"
  bottom: "inception_3a/5x5"
  bottom: "inception_3a/pool_proj"
  top: "inception_3a/output"
}

layer {
  name: "inception_3b/1x1"
  type: "Convolution"
  bottom: "inception_3a/output"
  top: "inception_3b/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_3b/relu_1x1"
  type: "ReLU"
  bottom: "inception_3b/1x1"
  top: "inception_3b/1x1"
}

layer {
  name: "inception_3b/3x3_reduce"
  type: "Convolution"
  bottom: "inception_3a/output"
  top: "inception_3b/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3b/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_3b/3x3_reduce"
  top: "inception_3b/3x3_reduce"
}
layer {
  name: "inception_3b/3x3"
  type: "Convolution"
  bottom: "inception_3b/3x3_reduce"
  top: "inception_3b/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 192
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3b/relu_3x3"
  type: "ReLU"
  bottom: "inception_3b/3x3"
  top: "inception_3b/3x3"
}

layer {
  name: "inception_3b/5x5_reduce"
  type: "Convolution"
  bottom: "inception_3a/output"
  top: "inception_3b/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3b/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_3b/5x5_reduce"
  top: "inception_3b/5x5_reduce"
}
layer {
  name: "inception_3b/5x5"
  type: "Convolution"
  bottom: "inception_3b/5x5_reduce"
  top: "inception_3b/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 96
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3b/relu_5x5"
  type: "ReLU"
  bottom: "inception_3b/5x5"
  top: "inception_3b/5x5"
}

layer {
  name: "inception_3b/pool"
  type: "Pooling"
  bottom: "inception_3a/output"
  top: "inception_3b/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_3b/pool_proj"
  type: "Convolution"
  bottom: "inception_3b/pool"
  top: "inception_3b/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_3b/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_3b/pool_proj"
  top: "inception_3b/pool_proj"
}
layer {
  name: "inception_3b/output"
  type: "Concat"
  bottom: "inception_3b/1x1"
  bottom: "inception_3b/3x3"
  bottom: "inception_3b/5x5"
  bottom: "inception_3b/pool_proj"
  top: "inception_3b/output"
}

layer {
  name: "pool3/3x3_s2"
  type: "Pooling"
  bottom: "inception_3b/output"
  top: "pool3/3x3_s2"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 2
  }
}

layer {
  name: "inception_4a/1x1"
  type: "Convolution"
  bottom: "pool3/3x3_s2"
  top: "inception_4a/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 192
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4a/relu_1x1"
  type: "ReLU"
  bottom: "inception_4a/1x1"
  top: "inception_4a/1x1"
}

layer {
  name: "inception_4a/3x3_reduce"
  type: "Convolution"
  bottom: "pool3/3x3_s2"
  top: "inception_4a/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 96
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4a/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_4a/3x3_reduce"
  top: "inception_4a/3x3_reduce"
}

layer {
  name: "inception_4a/3x3"
  type: "Convolution"
  bottom: "inception_4a/3x3_reduce"
  top: "inception_4a/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 208
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4a/relu_3x3"
  type: "ReLU"
  bottom: "inception_4a/3x3"
  top: "inception_4a/3x3"
}

layer {
  name: "inception_4a/5x5_reduce"
  type: "Convolution"
  bottom: "pool3/3x3_s2"
  top: "inception_4a/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 16
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4a/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_4a/5x5_reduce"
  top: "inception_4a/5x5_reduce"
}
layer {
  name: "inception_4a/5x5"
  type: "Convolution"
  bottom: "inception_4a/5x5_reduce"
  top: "inception_4a/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 48
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4a/relu_5x5"
  type: "ReLU"
  bottom: "inception_4a/5x5"
  top: "inception_4a/5x5"
}
layer {
  name: "inception_4a/pool"
  type: "Pooling"
  bottom: "pool3/3x3_s2"
  top: "inception_4a/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_4a/pool_proj"
  type: "Convolution"
  bottom: "inception_4a/pool"
  top: "inception_4a/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4a/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_4a/pool_proj"
  top: "inception_4a/pool_proj"
}
layer {
  name: "inception_4a/output"
  type: "Concat"
  bottom: "inception_4a/1x1"
  bottom: "inception_4a/3x3"
  bottom: "inception_4a/5x5"
  bottom: "inception_4a/pool_proj"
  top: "inception_4a/output"
}

layer {
  name: "inception_4b/1x1"
  type: "Convolution"
  bottom: "inception_4a/output"
  top: "inception_4b/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 160
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4b/relu_1x1"
  type: "ReLU"
  bottom: "inception_4b/1x1"
  top: "inception_4b/1x1"
}
layer {
  name: "inception_4b/3x3_reduce"
  type: "Convolution"
  bottom: "inception_4a/output"
  top: "inception_4b/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 112
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4b/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_4b/3x3_reduce"
  top: "inception_4b/3x3_reduce"
}
layer {
  name: "inception_4b/3x3"
  type: "Convolution"
  bottom: "inception_4b/3x3_reduce"
  top: "inception_4b/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 224
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4b/relu_3x3"
  type: "ReLU"
  bottom: "inception_4b/3x3"
  top: "inception_4b/3x3"
}
layer {
  name: "inception_4b/5x5_reduce"
  type: "Convolution"
  bottom: "inception_4a/output"
  top: "inception_4b/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 24
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4b/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_4b/5x5_reduce"
  top: "inception_4b/5x5_reduce"
}
layer {
  name: "inception_4b/5x5"
  type: "Convolution"
  bottom: "inception_4b/5x5_reduce"
  top: "inception_4b/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4b/relu_5x5"
  type: "ReLU"
  bottom: "inception_4b/5x5"
  top: "inception_4b/5x5"
}
layer {
  name: "inception_4b/pool"
  type: "Pooling"
  bottom: "inception_4a/output"
  top: "inception_4b/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_4b/pool_proj"
  type: "Convolution"
  bottom: "inception_4b/pool"
  top: "inception_4b/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4b/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_4b/pool_proj"
  top: "inception_4b/pool_proj"
}
layer {
  name: "inception_4b/output"
  type: "Concat"
  bottom: "inception_4b/1x1"
  bottom: "inception_4b/3x3"
  bottom: "inception_4b/5x5"
  bottom: "inception_4b/pool_proj"
  top: "inception_4b/output"
}

layer {
  name: "inception_4c/1x1"
  type: "Convolution"
  bottom: "inception_4b/output"
  top: "inception_4c/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4c/relu_1x1"
  type: "ReLU"
  bottom: "inception_4c/1x1"
  top: "inception_4c/1x1"
}

layer {
  name: "inception_4c/3x3_reduce"
  type: "Convolution"
  bottom: "inception_4b/output"
  top: "inception_4c/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}

layer {
  name: "inception_4c/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_4c/3x3_reduce"
  top: "inception_4c/3x3_reduce"
}
layer {
  name: "inception_4c/3x3"
  type: "Convolution"
  bottom: "inception_4c/3x3_reduce"
  top: "inception_4c/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 256
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4c/relu_3x3"
  type: "ReLU"
  bottom: "inception_4c/3x3"
  top: "inception_4c/3x3"
}
layer {
  name: "inception_4c/5x5_reduce"
  type: "Convolution"
  bottom: "inception_4b/output"
  top: "inception_4c/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 24
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4c/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_4c/5x5_reduce"
  top: "inception_4c/5x5_reduce"
}
layer {
  name: "inception_4c/5x5"
  type: "Convolution"
  bottom: "inception_4c/5x5_reduce"
  top: "inception_4c/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4c/relu_5x5"
  type: "ReLU"
  bottom: "inception_4c/5x5"
  top: "inception_4c/5x5"
}
layer {
  name: "inception_4c/pool"
  type: "Pooling"
  bottom: "inception_4b/output"
  top: "inception_4c/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_4c/pool_proj"
  type: "Convolution"
  bottom: "inception_4c/pool"
  top: "inception_4c/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4c/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_4c/pool_proj"
  top: "inception_4c/pool_proj"
}
layer {
  name: "inception_4c/output"
  type: "Concat"
  bottom: "inception_4c/1x1"
  bottom: "inception_4c/3x3"
  bottom: "inception_4c/5x5"
  bottom: "inception_4c/pool_proj"
  top: "inception_4c/output"
}

layer {
  name: "inception_4d/1x1"
  type: "Convolution"
  bottom: "inception_4c/output"
  top: "inception_4d/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 112
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_1x1"
  type: "ReLU"
  bottom: "inception_4d/1x1"
  top: "inception_4d/1x1"
}
layer {
  name: "inception_4d/3x3_reduce"
  type: "Convolution"
  bottom: "inception_4c/output"
  top: "inception_4d/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 144
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_4d/3x3_reduce"
  top: "inception_4d/3x3_reduce"
}
layer {
  name: "inception_4d/3x3"
  type: "Convolution"
  bottom: "inception_4d/3x3_reduce"
  top: "inception_4d/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 288
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_3x3"
  type: "ReLU"
  bottom: "inception_4d/3x3"
  top: "inception_4d/3x3"
}
layer {
  name: "inception_4d/5x5_reduce"
  type: "Convolution"
  bottom: "inception_4c/output"
  top: "inception_4d/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_4d/5x5_reduce"
  top: "inception_4d/5x5_reduce"
}
layer {
  name: "inception_4d/5x5"
  type: "Convolution"
  bottom: "inception_4d/5x5_reduce"
  top: "inception_4d/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_5x5"
  type: "ReLU"
  bottom: "inception_4d/5x5"
  top: "inception_4d/5x5"
}
layer {
  name: "inception_4d/pool"
  type: "Pooling"
  bottom: "inception_4c/output"
  top: "inception_4d/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_4d/pool_proj"
  type: "Convolution"
  bottom: "inception_4d/pool"
  top: "inception_4d/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 64
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4d/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_4d/pool_proj"
  top: "inception_4d/pool_proj"
}
layer {
  name: "inception_4d/output"
  type: "Concat"
  bottom: "inception_4d/1x1"
  bottom: "inception_4d/3x3"
  bottom: "inception_4d/5x5"
  bottom: "inception_4d/pool_proj"
  top: "inception_4d/output"
}

layer {
  name: "inception_4e/1x1"
  type: "Convolution"
  bottom: "inception_4d/output"
  top: "inception_4e/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 256
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_1x1"
  type: "ReLU"
  bottom: "inception_4e/1x1"
  top: "inception_4e/1x1"
}
layer {
  name: "inception_4e/3x3_reduce"
  type: "Convolution"
  bottom: "inception_4d/output"
  top: "inception_4e/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 160
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_4e/3x3_reduce"
  top: "inception_4e/3x3_reduce"
}
layer {
  name: "inception_4e/3x3"
  type: "Convolution"
  bottom: "inception_4e/3x3_reduce"
  top: "inception_4e/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 320
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_3x3"
  type: "ReLU"
  bottom: "inception_4e/3x3"
  top: "inception_4e/3x3"
}
layer {
  name: "inception_4e/5x5_reduce"
  type: "Convolution"
  bottom: "inception_4d/output"
  top: "inception_4e/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_4e/5x5_reduce"
  top: "inception_4e/5x5_reduce"
}
layer {
  name: "inception_4e/5x5"
  type: "Convolution"
  bottom: "inception_4e/5x5_reduce"
  top: "inception_4e/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_5x5"
  type: "ReLU"
  bottom: "inception_4e/5x5"
  top: "inception_4e/5x5"
}
layer {
  name: "inception_4e/pool"
  type: "Pooling"
  bottom: "inception_4d/output"
  top: "inception_4e/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_4e/pool_proj"
  type: "Convolution"
  bottom: "inception_4e/pool"
  top: "inception_4e/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_4e/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_4e/pool_proj"
  top: "inception_4e/pool_proj"
}
layer {
  name: "inception_4e/output"
  type: "Concat"
  bottom: "inception_4e/1x1"
  bottom: "inception_4e/3x3"
  bottom: "inception_4e/5x5"
  bottom: "inception_4e/pool_proj"
  top: "inception_4e/output"
}



layer {
  name: "inception_5a/1x1"
  type: "Convolution"
  bottom: "inception_4e/output"
  top: "inception_5a/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 256
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_1x1"
  type: "ReLU"
  bottom: "inception_5a/1x1"
  top: "inception_5a/1x1"
}

layer {
  name: "inception_5a/3x3_reduce"
  type: "Convolution"
  bottom: "inception_4e/output"
  top: "inception_5a/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 160
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.09
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_5a/3x3_reduce"
  top: "inception_5a/3x3_reduce"
}

layer {
  name: "inception_5a/3x3"
  type: "Convolution"
  bottom: "inception_5a/3x3_reduce"
  top: "inception_5a/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 320
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_3x3"
  type: "ReLU"
  bottom: "inception_5a/3x3"
  top: "inception_5a/3x3"
}
layer {
  name: "inception_5a/5x5_reduce"
  type: "Convolution"
  bottom: "inception_4e/output"
  top: "inception_5a/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 32
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.2
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_5a/5x5_reduce"
  top: "inception_5a/5x5_reduce"
}
layer {
  name: "inception_5a/5x5"
  type: "Convolution"
  bottom: "inception_5a/5x5_reduce"
  top: "inception_5a/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_5x5"
  type: "ReLU"
  bottom: "inception_5a/5x5"
  top: "inception_5a/5x5"
}
layer {
  name: "inception_5a/pool"
  type: "Pooling"
  bottom: "inception_4e/output"
  top: "inception_5a/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_5a/pool_proj"
  type: "Convolution"
  bottom: "inception_5a/pool"
  top: "inception_5a/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5a/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_5a/pool_proj"
  top: "inception_5a/pool_proj"
}
layer {
  name: "inception_5a/output"
  type: "Concat"
  bottom: "inception_5a/1x1"
  bottom: "inception_5a/3x3"
  bottom: "inception_5a/5x5"
  bottom: "inception_5a/pool_proj"
  top: "inception_5a/output"
}

layer {
  name: "inception_5b/1x1"
  type: "Convolution"
  bottom: "inception_5a/output"
  top: "inception_5b/1x1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 384
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_1x1"
  type: "ReLU"
  bottom: "inception_5b/1x1"
  top: "inception_5b/1x1"
}
layer {
  name: "inception_5b/3x3_reduce"
  type: "Convolution"
  bottom: "inception_5a/output"
  top: "inception_5b/3x3_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 1
    decay_mult: 0
  }
  convolution_param {
    num_output: 192
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_3x3_reduce"
  type: "ReLU"
  bottom: "inception_5b/3x3_reduce"
  top: "inception_5b/3x3_reduce"
}
layer {
  name: "inception_5b/3x3"
  type: "Convolution"
  bottom: "inception_5b/3x3_reduce"
  top: "inception_5b/3x3"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 384
    pad: 1
    kernel_size: 3
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_3x3"
  type: "ReLU"
  bottom: "inception_5b/3x3"
  top: "inception_5b/3x3"
}
layer {
  name: "inception_5b/5x5_reduce"
  type: "Convolution"
  bottom: "inception_5a/output"
  top: "inception_5b/5x5_reduce"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 48
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_5x5_reduce"
  type: "ReLU"
  bottom: "inception_5b/5x5_reduce"
  top: "inception_5b/5x5_reduce"
}
layer {
  name: "inception_5b/5x5"
  type: "Convolution"
  bottom: "inception_5b/5x5_reduce"
  top: "inception_5b/5x5"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    pad: 2
    kernel_size: 5
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_5x5"
  type: "ReLU"
  bottom: "inception_5b/5x5"
  top: "inception_5b/5x5"
}
layer {
  name: "inception_5b/pool"
  type: "Pooling"
  bottom: "inception_5a/output"
  top: "inception_5b/pool"
  pooling_param {
    pool: MAX
    kernel_size: 3
    stride: 1
    pad: 1
  }
}
layer {
  name: "inception_5b/pool_proj"
  type: "Convolution"
  bottom: "inception_5b/pool"
  top: "inception_5b/pool_proj"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 128
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.1
    }
    bias_filler {
      type: "constant"
      value: 0.2
    }
  }
}
layer {
  name: "inception_5b/relu_pool_proj"
  type: "ReLU"
  bottom: "inception_5b/pool_proj"
  top: "inception_5b/pool_proj"
}
layer {
  name: "inception_5b/output"
  type: "Concat"
  bottom: "inception_5b/1x1"
  bottom: "inception_5b/3x3"
  bottom: "inception_5b/5x5"
  bottom: "inception_5b/pool_proj"
  top: "inception_5b/output"
}
layer {
  name: "pool5/drop_s1"
  type: "Dropout"
  bottom: "inception_5b/output"
  top: "pool5/drop_s1"
  dropout_param {
    dropout_ratio: 0.4
  }
}
layer {
  name: "cvg/classifier"
  type: "Convolution"
  bottom: "pool5/drop_s1"
  top: "cvg/classifier"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 1
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.
    }
  }
}
layer {
  name: "coverage/sig"
  type: "Sigmoid"
  bottom: "cvg/classifier"
  top: "coverage"
}
layer {
  name: "bbox/regressor"
  type: "Convolution"
  bottom: "pool5/drop_s1"
  top: "bboxes"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  convolution_param {
    num_output: 4
    kernel_size: 1
    weight_filler {
      type: "xavier"
      std: 0.03
    }
    bias_filler {
      type: "constant"
      value: 0.
    }
  }
}

######################################################################
# End of convolutional network
######################################################################

# Convert bboxes
layer {
  name: "bbox_mask"
  type: "Eltwise"
  bottom: "bboxes"
  bottom: "coverage-block"
  top: "bboxes-masked"
  eltwise_param {
    operation: PROD
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "bbox-norm"
  type: "Eltwise"
  bottom: "bboxes-masked"
  bottom: "size-block"
  top: "bboxes-masked-norm"
  eltwise_param {
    operation: PROD
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "bbox-obj-norm"
  type: "Eltwise"
  bottom: "bboxes-masked-norm"
  bottom: "obj-block"
  top: "bboxes-obj-masked-norm"
  eltwise_param {
    operation: PROD
  }
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}

# Loss layers
layer {
  name: "bbox_loss"
  type: "L1Loss"
  bottom: "bboxes-obj-masked-norm"
  bottom: "bbox-obj-label-norm"
  top: "loss_bbox"
  loss_weight: 2
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}
layer {
  name: "coverage_loss"
  type: "EuclideanLoss"
  bottom: "coverage"
  bottom: "coverage-label"
  top: "loss_coverage"
  include { phase: TRAIN }
  include { phase: TEST stage: "val" }
}

# Cluster bboxes
layer {
    type: 'Python'
    name: 'cluster'
    bottom: 'coverage'
    bottom: 'bboxes'
    top: 'bbox-list'
    python_param {
        module: 'caffe.layers.detectnet.clustering'
        layer: 'ClusterDetections'
        param_str : '640, 640, 16, 0.6, 2, 0.02, 22, 1'
    }
    include: { phase: TEST }
}

# Calculate mean average precision
layer {
  type: 'Python'
  name: 'cluster_gt'
  bottom: 'coverage-label'
  bottom: 'bbox-label'
  top: 'bbox-list-label'
  python_param {
      module: 'caffe.layers.detectnet.clustering'
      layer: 'ClusterGroundtruth'
      param_str : '640, 640, 16, 1'
  }
  include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'score'
    bottom: 'bbox-list-label'
    bottom: 'bbox-list'
    top: 'bbox-list-scored'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'ScoreDetections'
    }
    include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'mAP'
    bottom: 'bbox-list-scored'
    top: 'mAP'
    top: 'precision'
    top: 'recall'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'mAP'
        param_str : '640, 640, 16'
    }
    include: { phase: TEST stage: "val" }
}
```

Luckily for you, the model architecture is already coded (hurray). You just have to set up some lines depending of your situation. Let's see them.

```
layer {
  name: "deploy_data"
  type: "Input"
  top: "data"
  input_param {
    shape {
      dim: 1
      dim: 3
      dim: 480
      dim: 720
    }
  }
```

You have to adjust the size to fit with your dataset images sizes like in the example up here. They appear in line, **57-59,79-80,118-119,2504,2519 and 2545.**

Multiclass model

If you want to make a multiclass model, some other changes are to considerate in the prototxt file to make a good model.

First, you have to specify each class you want in line **<u>82 and 121</u>.**

```
object_class: { src: 1 dst: 0} # obj class 1 -> cvg index 0
  }
```

become this line :

```
object_class: { src: 1 dst: 0} # your first object
object_class: { src: 2 dst: 1} # your second object
```

You add the number of *object\_class* depending of how much you have objects to identify. Personally, I haven't tested with more than two objects.

Another change to made is in lines **<u>2502 and 2504</u>.**

```
 top: 'bbox-list'
    python_param {
        module: 'caffe.layers.detectnet.clustering'
        layer: 'ClusterDetections'
        param_str : '720, 480, 16, 0.6, 2, 0.02, 22, 1'
    }
```

become this line :

```
top: 'bbox-list-class0'
top: 'bbox-list-class1'
   python_param {
        module: 'caffe.layers.detectnet.clustering'
        layer: 'ClusterDetections'
        param_str : '720, 480, 16, 0.6, 2, 0.02, 22, 2'
    }
```

Now lines <u>**2515 and 2518.**</u>

```
top: 'bbox-list-label'
  python_param {
      module: 'caffe.layers.detectnet.clustering'
      layer: 'ClusterGroundtruth'
      param_str : '720, 480, 16, 1'
  }
```

become this :

```
top: 'bbox-list-label-class0'
top: 'bbox-list-label-class1'
  python_param {
      module: 'caffe.layers.detectnet.clustering'
      layer: 'ClusterGroundtruth'
      param_str : '720, 480, 16, 2'
  }
```

Final step ! These lines **<u>2529 to the end</u>.**

```
layer {
    type: 'Python'
    name: 'score'
    bottom: 'bbox-list-label'
    bottom: 'bbox-list'
    top: 'bbox-list-scored'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'ScoreDetections'
    }
    include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'mAP'
    bottom: 'bbox-list-scored'
    top: 'mAP'
    top: 'precision'
    top: 'recall'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'mAP'
        param_str : '640, 640, 16'
}
    include: { phase: TEST stage: "val" }
}
```

become this :

```
layer {
    type: 'Python'
    name: 'score-class0'
    bottom: 'bbox-list-label-class0'
    bottom: 'bbox-list-class0'
    top: 'bbox-list-scored-class0'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'ScoreDetections'
    }
    include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'mAP-class0'
    bottom: 'bbox-list-scored-class0'
    top: 'mAP-class0'
    top: 'precision-class0'
    top: 'recall-class0'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'mAP'
        param_str : '720, 480, 16'
}
    include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'score-class1'
    bottom: 'bbox-list-label-class1'
    bottom: 'bbox-list-class1'
    top: 'bbox-list-scored-class1'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'ScoreDetections'
    }
    include: { phase: TEST stage: "val" }
}
layer {
    type: 'Python'
    name: 'mAP-class1'
    bottom: 'bbox-list-scored-class1'
    top: 'mAP-class1'
    top: 'precision-class1'
    top: 'recall-class1'
    python_param {
        module: 'caffe.layers.detectnet.mean_ap'
        layer: 'mAP'
        param_str : '720, 480, 16'
}
    include: { phase: TEST stage: "val" }
}
```

Don't forget to specifiy **the path of your prototxt file** for your model.

Finally, select **how many GPU(s)** you want to use for your training and**name your model**.

![Aucune description disponible.](https://scontent.fymy1-1.fna.fbcdn.net/v/t1.15752-9/123944375_733980323871830_5955319504382384956_n.png?_nc_cat=107&ccb=2&_nc_sid=ae9488&_nc_ohc=jd5QRhJdR14AX89wHCF&_nc_ht=scontent.fymy1-1.fna&oh=5c28846118eb2c12e6f95157d42995e2&oe=603B62B1)

Once everything is set, click on **Create** and that's where the magic start.

## 2.3 Training phase

At this phase, you don't have really something to do. Just wait until the training is done and keep an eye on precision curves to be sure all your settings are correct. If curves do not up **after 25-30 epochs**, your settings are not correct or something went wrong.

Time of a training depends of many things :

* Size and resolution of your dataset
* GPU memory
* Number of epochs
* Batch size/accumulation

*Digits* provides curves to help you visualize your training.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127637803_445396296447429_8181304574838859238_n.png?_nc_cat=100&ccb=2&_nc_sid=ae9488&_nc_ohc=2jM4n4QTmUkAX_75TXu&_nc_ht=scontent.fymy1-2.fna&oh=e5199e2e3420faf4acfb646063c8597a&oe=603A03D1)

Here's the definition of each curve :

* **loss\_bbox** is the mean absolute difference of the true and predicted corners of the bounding box for the object covered by each grid square.
* **loss\_coverage** value of 0 or 1 indicates whether an object is present within the grid square. It is defined as the sum of squares of differences between the true and predicted object coverage across all grid squares in a training data sample.
* **Precision** is the ratio of the accurately identified objects to the total number of predicted objects (ratio of true positives to true positives plus false positives).
* **Recall** is the ratio of the accurately identified objects to the total number of actual objects in the images (ratio of true positives to true positives plus false negatives).
* **mAP:** a simplified mean Average Precision score based on the product of the precision and recall for DetectNet. It’s a good combined measure for how sensitive the network is to objects of interest and how well it avoids false alarms.

The most important values here are **mAP** and **Precision**. These values tell how strong is your network for object detection.

You can also visualize the curve of your learning rate during the training :

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127810410_374010203659916_8975661302473034898_n.png?_nc_cat=106&ccb=2&_nc_sid=ae9488&_nc_ohc=BBSQehFG484AX86zWO4&_nc_ht=scontent.fymy1-2.fna&oh=464168fccd96d6be9927a736056586f2&oe=60385B31)

Once a epoch is done, you can test it with Jetson Inference (models reader made by NVIDIA) directly in *Digits.*

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127890988_210676950448385_2300438285564145944_n.png?_nc_cat=103&ccb=2&_nc_sid=ae9488&_nc_ohc=S1GtJXGQP10AX9-YUUw&_nc_ht=scontent.fymy1-2.fna&oh=800b1a5a16b5a4cb896980518fc312f0&oe=6038698A)

Just select the epoch you want to test. For better results, use the one with higher precision value. Don't forget to select bounding boxes in visualization method to add boxes around your input image you want to test.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127603913_388752732363680_2594530769283553215_n.png?_nc_cat=106&ccb=2&_nc_sid=ae9488&_nc_ohc=UbWIQtEopbQAX8hRMSm&_nc_ht=scontent.fymy1-2.fna&oh=967856795a3b1d6bc465a11d737ee2e6&oe=603AA36C)

Add your image for test here. For more informations about each layers of your network and how your model actually work, you can activate the Show visualizations and statistics option. Now, it's time to test.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/128019216_417161246326041_6222078275293795162_n.png?_nc_cat=105&ccb=2&_nc_sid=ae9488&_nc_ohc=PVPUbIA7AtgAX-GftOp&_nc_ht=scontent.fymy1-2.fna&oh=a06c04c6e94cf3ab91a475d307186c4e&oe=60389B18)

Another window appears with the result of the test. You can see the model is pretty accurate here. It's a good way to verify your model is well trained.

With statistics option activate, other infos on each layers of the network like these appears :

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127518623_809499049892378_3545136807599886842_n.png?_nc_cat=104&ccb=2&_nc_sid=ae9488&_nc_ohc=UEDg_301D0MAX8LNADW&_nc_ht=scontent.fymy1-2.fna&oh=ad505c1904637e0071bf8fe6231e6ecd&oe=60394F9F)

### 2.3.1 Retraining

Sometimes, at the end of a training, it's look like the precision doesn't have finish to stabilize because the number of epochs wasn't enough. If this occurs, you can retrain your model instead of restarting from zero your training. To do that, select the last epoch of your model and click on Make Petrained model.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127890988_210676950448385_2300438285564145944_n.png?_nc_cat=103&ccb=2&_nc_sid=ae9488&_nc_ohc=S1GtJXGQP10AX9-YUUw&_nc_ht=scontent.fymy1-2.fna&oh=800b1a5a16b5a4cb896980518fc312f0&oe=6038698A)

Just set up a new training like we see in the steps before. Instead of a custom network, select your pretrained model to continue training it.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127678894_196756375254100_794751442036322620_n.png?_nc_cat=109&ccb=2&_nc_sid=ae9488&_nc_ohc=dMawrpqmjcYAX8SwmYc&_nc_oc=AQmZho1FSDoGcUruIHvnK7cDATKJ8F9Rh7XqKTkpzdC0dTYLIcokBDR2zSmiQNcCgpU&_nc_ht=scontent.fymy1-2.fna&oh=350c36ff084b45a90ba061b91782a82d&oe=603B15D3)

Start the training and it will continue where it was at the end. It's also a good technique to **evaluate your model cannot be higher in precision**.

## 2.4 Export your model

Once precision of your trained model looks good, it's time to export it from *Digits* to use it on the Jetson Xavier. To export your model, select the epoch with the best precision and click on download model.

![Aucune description disponible.](https://scontent.fymy1-2.fna.fbcdn.net/v/t1.15752-9/127890988_210676950448385_2300438285564145944_n.png?_nc_cat=103&ccb=2&_nc_sid=ae9488&_nc_ohc=S1GtJXGQP10AX9-YUUw&_nc_ht=scontent.fymy1-2.fna&oh=800b1a5a16b5a4cb896980518fc312f0&oe=6038698A)

It will produces a zip file with all files necessary to use this model.

And that's it for the training and *Digits.* Feel free to contact me if you have questions or problems during your training with this tool.

* Page:

[How to use a custom trained model in Jetson-Inference](/display/SUBUQTR/How+to+use+a+custom+trained+model+in+Jetson-Inference)
* Page:

[How to train a model with NVIDIA Digits](/display/SUBUQTR/How+to+train+a+model+with+NVIDIA+Digits)
* Page:

[How to create a dataset](/display/SUBUQTR/How+to+create+a+dataset)
* Page:

[Getting Started : Deep Learning](/display/SUBUQTR/Getting+Started+%3A+Deep+Learning)

## Attachments:

![](images/icons/bullet_blue.gif) [image2020-11-4\_14-50-40.png](attachments/42139672/42139674.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-4\_14-52-38.png](attachments/42139672/42139675.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-4\_14-55-18.png](attachments/42139672/42139676.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_13-54-30.png](attachments/42139672/42139685.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_13-54-44.png](attachments/42139672/42139686.png) (image/png)  
![](images/icons/bullet_blue.gif) [image2020-11-5\_13-55-5.png](attachments/42139672/42139687.png) (image/png)

Document generated by Confluence on May 02, 2023 17:22

[Atlassian](https://www.atlassian.com/)