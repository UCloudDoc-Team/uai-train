{{indexmenu_n>2}}

# 生成tfrecord文件

### FER2013数据集介绍

FER2013数据集是一个人脸表情识别数据集，该数据集中的图片共有七种类别，分别是angry、disgust、fear、happy、neutral、sad、surprise。该数据集中共有35886张图片，每张图片的大小为48×48×1。FER2013数据集的存放结构如下所示：

    |_ fer/pic/
      |_ angry\
         |_ 00000_1.jpg
         |_ ...
      |_ disgust\
      |_ fear\
      |_ happy\
      |_ neutral\
      |_ sad\
      |_ surprise\

### 生成docker镜像

我们需要生成一个docker镜像用于后续的（1）生成tfrecord文件（2）训练模型（3）模型评估。

你可以根据以下步骤生成一个docker镜像，也可以跳过此步骤，使用本文提供的镜像。

通过以下命令拉取本文提供的镜像,并重命名为自己的镜像：

    sudo docker pull uhub.service.ucloud.cn/uai_demo/slim:latest
    sudo docker tag uhub.service.ucloud.cn/uai_demo/slim:latest uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/slim

\*\* - 1 TF-Slim代码准备\*\*

我们可以在[UCloud-github](https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/slim)中获得所需代码。

\*\* - 2 Dockerfile文件准备\*\*

我们使用tensorflow 1.5（tf-models
1.8.0）作为基础镜像，将slim文件夹添加到镜像的/data/文件夹下。Dockerfile文件如下：

    FROM uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-16.04_python-2.7.6_tensorflow-1.5_models:v1.8.0
    COPY ./slim/ /data/

\*\* - 3 生成docker镜像\*\*

经过以上准备，我们本地的文件结构应该为如下格式：

``` 
|_ data\
   |_ slim\
   |_ slim.Dockerfile  
```

通过如下命令，可以在本地生成一个名为uhub.ucloud.cn/\<YOUR\\\_UHUB\\\_REGISTRY\>/slim的docker镜像；

    sudo docker build -t uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/slim -f slim.Dockerfile .

### 生成tfrecord文件

我们需要将fer2013的图片文件转化为tfrecord文件，方便我们进行后续的模型训练。

我们将会生成两个tfrecord文件，分别为fer\\\_test.tfrecord、fer\\\_train.tfrecord，其中fer\\\_test.tfrecord包含了原数据集30%的图片，fer\\\_train.tfrecord包含了原数据集70%的图片。
在生成tfrecord文件的同时，我们还会生成info.json和labels.txt，他们储存了fer2013的类别信息和tfrecord文件的相关信息。

运行以下命令，可以在/data/fer/tfrecord下得到生成的tfrecord文件、info.json和labels.txt。

    sudo nvidia-docker run -it -v /data/fer/pic:/data/pic -v /data/fer/tfrecord:/data/tfrecord /bin/bash -c "python /data/download_and_convert_data.py --dataset_dir=/data/tfrecord --dataset_name=fer --pic_path=/data/pic"
