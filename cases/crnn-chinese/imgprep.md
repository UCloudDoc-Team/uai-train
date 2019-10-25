

# 生成镜像
我们需要一个docker镜像用于后续的数据文件准备，你可以通过下列步骤自己生成一个镜像，同时我们提供了一个已经做好的镜像，你可以在云主机上通过docker pull命令下载该镜像。
如果你不打算自己生成镜像，则可以跳过下面的“数据准备”，“Dockerfile文件”和“生成镜像”这两个步骤。

你可以通过下列命令下载我们提供的镜像。
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest
</code>

此外，我们还提供了一个gpu版本的镜像用于UAI-Train训练平台使用，你可以通过下列命令下载镜像。
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/crnn_gpu:latest
</code>

## 数据准备
我们可以将数据按照如下文件结构进行放置。你可以在[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/crnn|ucloud/uai-sdk/example/tensorflow/train/crnn]]中找到下面的代码文件。

  * ./code中放置了相应的代码文件；
<code>

/_ build/
  |_ code/
    |_ crnn_model/
    |_ data_provoder/
    |_ global_configuration/
    |_ local_utils/
    |_ tools/
    |_ gen_data/
  |_ crnn-cpu.Dockerfile
  |_ crnn-gpu.Dockerfile
</code>

### Dockerfile文件
Docker镜像build的时候会基于uhub.service.ucloud.cn/uaishare/cpu\_uaitrain\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.3.0:v1.0的基础镜像，然后将code/下面的代码拷贝到docker镜像的/data/code/目录下，具体的crnn-cpu.Dockerfile文件的代码如下：
<code>
From uhub.service.ucloud.cn/uaishare/cpu_uaitrain_ubuntu-16.04_python-3.6.2_tensorflow-1.3.0:v1.0

RUN pip install tqdm
ENV LANG C.UTF-8
ADD ./code/ /data/code/
</code>

## 生成镜像
我们通过build命令来生成Docker镜像：uhub.service.ucloud.cn/uai\_demo/crnn\_cpu:latest，你需要将“uai\_demo”修改为 <YOUR\_UHUB\_REGISTRY>.
首先进入build文件夹：
<code>
sudo docker build -t uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest -f crnn-cpu.Dockerfile .
</code>
你可以通过crnn-gpu.Dockerfile文件生成gpu镜像：
<code>
sudo docker build -t uhub.service.ucloud.cn/uai_demo/crnn_gpu:latest -f crnn-gpu.Dockerfile .
</code>
