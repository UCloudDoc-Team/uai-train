{{indexmenu_n>7}}

# MNIST开发案例
本案例所使用的模型和代码基于Caffe 的MNIST案例，您可以在https://github.com/BVLC/caffe/tree/master/examples/mnist找到Caffe原始的案例。您可以在您可以在https://github.com/ucloud/uai-sdk/tree/master/examples/caffe/train/mnist/下面下载完整的代码。

## 准备工作
请根据 开发指南>Caffe开发指南>Caffe 本地安装部署开发环境[[ai:uai-train:guide:caffe:local]]完成所有安装步骤，即完成了基本环境的部署 
从https://github.com/ucloud/uai-sdk/tree/master/examples/caffe/train/mnist/下载完整的案例，包括：

  * code/ 训练代码，包括train.py,  lenet\_solver.prototxt, lenet\_train\_test.prototxt
  * data/ 测试数据集, 由于测试数据比较大没有上传github，可以从以下地址下载(需要建立mnist\_train\_lmdb和mnist\_test\_lmdb两个子目录来分别存放train和test数据)：
<code>
mnist_train_lmdb/lock.mdb: http://uai-train-image.cn-bj.ufileos.com/caffe/mnist/data/mnist_train_lmdb/lock.mdb
  * mnist_train_lmdb/data.mdb: http://uai-train-image.cn-bj.ufileos.com/caffe/mnist/data/mnist_train_lmdb/data.mdb
mnist_test_lmdb/lock.mdb: http://uai-train-image.cn-bj.ufileos.com/caffe/mnist/data/mnist_test_lmdb/lock.mdb
mnist_test_lmdb/data.mdb: http://uai-train-image.cn-bj.ufileos.com/caffe/mnist/data/mnist_test_lmdb/data.mdb
</code>

## 编写MNIST案例
本案例使用的MNIST模型的训练描述文件来自Caffe的MNIST案例的 lenet\_solver.prototxt, lenet\_train\_test.prototxt, 并做如下修改 

### train.py
由于UAI-Train平台统一采用python的接口启动训练任务，因此我们需要该train.py 文件作为入口 ，所以需要将uai-sdk/uaitrain/arch/caffe/train.py 拷贝至代码目录（案例中，我们已经在mnist/code/ 目录下准备好了。

### lenet_solver.prototxt

#### 指定mnist网络文件的路径
UAI-Train平台在训练过程中，随Docker镜像打包（打包方法请见[[ai:uai-train:guide:caffe:packing]]）进训练镜像的代码在/data/目录下，数据则会指定下载到/data/data目录下。指定的网络文件可以有两种存放路径：

  - /data/, 此时网络定义文件lenet\_train\_test.prototxt需要同train.py 一同打包进/data 目录下。
  - /data/data/，此时此时网络定义文件lenet\_train\_test.prototxt需要和数据一同上传至UFIle存储中，训练过程文件将下载至/data/data/目录下。**用户可以通过该方案，在不重新打包Docker镜像的基础上，动态修改网络定义的文件**

案例中，我们采用了第一种方案，将**net**指定的网络定义文件的路径改为了**/data/lenet\_train\_test.prototxt**。（注：需要将lenet\_train\_test.prototxt同train.py放在同一个目录下打包进训练用的docker镜像中）
<code>
net: "examples/mnist/lenet_train_test.prototxt"  -> net: "/data/lenet_train_test.prototxt"
</code>

用户也可以使用第二种方案，将**net**指定的网络定义文件路径改为**/data/data/lenet\_train\_test.prototxt**, 并将lenet\_train\_test.prototxt同数据一同上传UFile

#### 指定snapshot 文件的路径

UAI Train平台在训练结束后会主动将/data/output目录下的数据上传到用户指定的UFIle路径下，因此**我们需要修改snapshot 文件的路路径至/data/output/路径下**
案例中，我们做了如下修改：
<code>
snapshot_prefix: "examples/mnist/lenet"  -> snapshot_prefix: "/data/output/"
</code>

### lenet_train_test.prototxt

UAI Train平台在训练过程中，会将UFile中的数据指定下载到/data/data目录下，因此我们需要对lenet\_train\_test.prototxt 做针对性的修改，主要修改的是data\_param的source路径， 包括Train数据集和Test数据集：
<code>
name: "LeNet"

layer {
  name: "mnist"
  type: "Data"
  top: "data"
  top: "label"
  include {
    phase: TRAIN
  }
  transform_param {
    scale: 0.00390625
  }
  data_param {
     source: "examples/mnist/mnist_train_lmdb"  ->  source: "/data/data/mnist_train_lmdb"
    batch_size: 64
    backend: LMDB
  }
}
layer {
  name: "mnist"
  type: "Data"
  top: "data"
  top: "label"
  include {
    phase: TEST
  }
  transform_param {
    scale: 0.00390625
  }
  data_param {
    source: "examples/mnist/mnist_test_lmdb"  ->  source: "/data/data/mnist_test_lmdb"
    batch_size: 100
    backend: LMDB
  }
}
</code>

## 测试MNIST案例

**确保本地已经安装Caffe 1.0.0 和 pyCaffe，并且将examples/caffe/train/mnist/data 下的测试数据放入 /data/data目录下，并确定/data/output 目录存在。**
执行以下代码执行训练任务
<code>
python train.py --solver=/data/lenet_solver.prototxt
</code>

## 使用Docker镜像进行测试

因为UAI Train 平台使用Docker运行训练，因此可以使用Docker对训练代码进行测试。
UAI Train平台提供开源的Docker 打包工具，打包方法见[[ai:uai-train:guide:caffe:packing]] 
打包完成后打包工具会自动生成GPU的镜像和CPU的镜像，命名如下：

  * **CPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>-cpu:<usr-def-tag>
  * **GPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>:<usr-def-tag>

### 使用CPU Docker 测试

打包工具会自动生成CPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for CPU local test: <docker run cmd> ）
直接执行命令即可

### 使用GPU Docker 测试

打包工具会自动生成GPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for GPU local test: <docker run cmd> ）
直接执行命令即可