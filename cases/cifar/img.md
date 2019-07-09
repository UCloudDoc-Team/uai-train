{{indexmenu_n>2}}

## 建立镜像

我们借助Docker镜像来进行模型训练。

我们已经提供了一个docker镜像用于模型训练，你可以在云主机上通过docker
pull命令下载该镜像，你也可以通过Dockerfile文件自己生成镜像。

### 使用我们提供的镜像

你可以通过下列命令下载本文提供的镜像,并跳过下面的“自己生成镜像”步骤：

    sudo dokcer pull uhub.service.ucloud.cn/uai_demo/cifar_gpu_train_simple:latest

你需要将该镜像重新docker
tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/\<YOUR\\\_UHUB\\\_REGISTRY\>/cifar\\\_gpu\\\_train\\\_simple:latest

### 自己生成镜像

我们使用Dockerfile直接打包，首先建立如下文件结构。

    /_ data/
      |_ code/
      |_ cifar-cpu.Dockerfile
      |_ cifar-gpu.Dockerfile

其中/code文件夹放置模型训练所需的代码文件，我们通过cifar-gpu.Dockerfile文件进行镜像打包。

#### cifar-gpu.Dockerfile

我们以uhub.service.ucloud.cn/uaishare/gpu\\\_uaitrain\\\_ubuntu-14.04\\\_python-2.7.6\\\_tensorflow-1.4.0:v1.0
作为基础镜像，将/code下的代码文件添加到/data/code/文件夹下。

    FROM uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.4.0:v1.0 
    ADD ./code/ /data/code/

#### build cifar镜像

我们使用docker build命令来创建一个gpu镜像

    sudo docker build -t uhub.service.ucloud.cn/YOUR_UHUB_REGISTRY/cifar_gpu_train_simple:latest -f cifar-gpu.Dockerfile .

可以得到一个名为uhub.service.ucloud.cn/YOUR\\\_UHUB\\\_REGISTRY/cifar\\\_gpu\\\_train\\\_simple:latest的docker镜像。

你也可以构建一个cpu镜像。

    sudo docker build -t uhub.service.ucloud.cn/YOUR_UHUB_REGISTRY/cifar_cpu_train_simple:latest -f cifar-cpu.Dockerfile .

可以得到一个名为uhub.service.ucloud.cn/YOUR\\\_UHUB\\\_REGISTRY/cifar\\\_cpu\\\_train\\\_simple:latest的docker镜像。

### 上传cifar镜像

我们将镜像上传到镜像容器库中：

上传cpu镜像：

    sudo docker push uhub.service.ucloud.cn/YOUR_UHUB_REGISTRY/cifar_cpu_train_simple:latest

上传gpu镜像：

    sudo docker push uhub.service.ucloud.cn/YOUR_UHUB_REGISTRY/cifar_gpu_train_simple:latest
