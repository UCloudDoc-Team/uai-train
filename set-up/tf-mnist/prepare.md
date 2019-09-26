{{indexmenu_n>2}}

# 环境准备
我们可以在CPU的云主机上为MNIST的训练做准备，该云主机需要满足以下条件:

  - 需要是Linux或类Linux环境
  - 安装docker，建议使用docker-ce，且版本 > 10.0
  - 该主机可以访问uhub.service.ucloud.cn 或 uhub.ucloud.cn

本Tutorial将以UCloud 普通云主机为范例操作（你也可以使用自己的主机或其他系统）

## 创建云主机
我们根据[[ai:uai-train:basic:ubuntu]]操作申请一台2核4G的CPU云主机作为操作平台。

### 安装docker
1.设置官方docker软件包源
<code>
sudo apt-get -y install \
  apt-transport-https \
  ca-certificates \
  curl

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

sudo apt-get update
</code>

2.安装docker-ce

<code>
sudo apt-get -y install docker-ce
</code>

3.测试docker安装

<code>
sudo docker run hello-world
</code>

## 安装UAI SDK
我们需要从github上面下载uai-sdk：
<code>
$ cd ~

$ git clone https://github.com/ucloud/uai-sdk.git
$ cd uai-sdk
$ sudo python setup.py install
</code>

## 准备操作环境
我们在/data/目录下创建一个mnist目录来执行操作：

<code>
$ cd /data/

$ mkdir mnist/
</code>

### 下载数据
我们可以从[[http://yann.lecun.com/exdb/mnist/]]下载Mnist的训练数据和测试数据，你也可以从[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/mnist_summary_1.1/data]]获取，我们将数据放入/data/mnist/data/路径：

<code>
$ mkdir /data/mnist/data/

$ cd /data/mnist/data/
$ wget http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
$ wget http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz
$ wget http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz
$ wget http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz
$ ls
train-images-idx3-ubyte.gz  train-labels-idx1-ubyte.gz  t10k-images-idx3-ubyte.gz  t10k-labels-idx1-ubyte.gz
</code>

### 下载代码
我们可以直接从[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/mnist_summary_1.1]]获取代码，并放入/data/mnist/code/路径。由于之前我们已经从github下载的uai-sdk，我们只需要去examples下面拷贝 tensorflow mnist例子的代码至/data/mnist下面即可：
<code>
$ cp -fr ~/uai-sdk/examples/tensorflow/train/mnist_summary_1.1/code/ /data/mnist/
</code>

## 环境Ready
至此，我们已经准备好了

  - 基础环境：docker
  - UAI-SDK环境：uai-sdk
  - 代码和数据
<code>
|_ data
  - |  |_ mnist
|  |  |_ code
|  |  |  |_ mnist_summary.py
|  |  |_ data
|  |  |  |_ train-images-idx3-ubyte.gz
|  |  |  |_ train-labels-idx1-ubyte.gz
|  |  |  |_ t10k-images-idx3-ubyte.gz
|  |  |  |_ t10k-labels-idx1-ubyte.gz
</code>

