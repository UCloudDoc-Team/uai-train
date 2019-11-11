

# 自定义镜像打包
我们同样也可以使用自定义的docker镜像作为基础镜像来打包mnist训练镜像，这里我们提供两种方式：

  - 使用UAI-SDK自定义打包工具
  - 使用Dockerfile直接打包

## 使用UAI-SDK自定义打包工具
UAI-SDK自定义打包工具为 uai-sdk/uaitrain\_tool/base\_tool.py，我们将该工具也放入/data/mnist。
<code>
$ cd /data/mnist

$ cp ~/uai-sdk/uaitrain_tool/base_tool.py ./
</code>

目前的目录结构如下：
<code>
|_ /data/mnist

|  |_ code
|  |_ data
|  |_ base_tool.py
</code>

### 打包Mnist镜像

我们使用base\_tool.py 打包mnist镜像，具体的参数说明在[](ai/uai-train/guide/scripts/self-pack)。
<code>
$ sudo python base_tool.py pack \

​			--code_path=<CODE_PATH> \
​			--mainfile_path=<MAIN_FUNC_FILE> \
​			--uhub_username=<YOUR_UHUB_USER_NAME> \
​			--uhub_password=<YOUR_UHUB_PASSWORD> \
​			--uhub_registry=<YOUR_UHUB_REFDISTRY> \
​			--uhub_imagename=<YOUR_UHUB_IMAGENAME> \
​            --internal_uhub=<true/false> \
​			--test_data_path=<LOCAL_DATA_PATH> \
​			--test_output_path=<LOCAL_OUTPUT_PATH> \
​			--train_params=<PARAMS> \
​			--self_img <YOUR_SOURCE_INAGE>
</code>

基本参数同[](ai/uai-train/set-up/tf-mnist/pack)中的一致，除了**ai\_arch\_v**参数（这里也不需要使用\-\-os 和\-\-python\_version参数）改为了**self\_img** 。
我们通过**self\_img**参数来指定基础docker镜像，并基于该镜像打包自己的mnist训练镜像。

### 打包CPU Mnist训练镜像

<code>
$ sudo python base_tool.py pack \

​			--code_path=./code/ \
​			--mainfile_path=mnist_summary.py \
​			--uhub_username=<YOUR_UHUB_USER_NAME> \
​			--uhub_password=<YOUR_UHUB_PASSWORD> \
​			--uhub_registry=uai_demo \
​			--uhub_imagename=tf-mnist-train-cpu \
​            --internal_uhub=true \
​			--test_data_path=/data/mnist/data \
​			--test_output_path=/data/mnist/output \
​			--train_params="--max_step=2000" \
​			--self_img=uhub.service.ucloud.cn/uaishare/cpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0
</code>
这样可以生成一个镜像名为 **tf-mnist-train-cpu:uaitrain** 的CPU Mnist训练镜像。

### 打包GPU Mnist训练镜像
<code>
$ sudo python base_tool.py pack \

​			--code_path=./code/ \
​			--mainfile_path=mnist_summary.py \
​			--uhub_username=<YOUR_UHUB_USER_NAME> \
​			--uhub_password=<YOUR_UHUB_PASSWORD> \
​			--uhub_registry=uai_demo \
​			--uhub_imagename=uhub.service.ucloud.cn/uai_demo/tf-mnist-train \
​            --internal_uhub=true \
​			--test_data_path=/data/mnist/data \
​			--test_output_path=/data/mnist/output \
​			--train_params="--max_step=2000" \
​			--self_img=uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0
</code>
这样可以生成一个镜像名为 **uhub.service.ucloud.cn/uai_demo/tf-mnist-train:uaitrain** 的GPU Mnist训练镜像。

## 使用Dockerfile直接打包
我们也可以直接使用Dockerfile来打包镜像，我们需要在/data/mnist下面创建一个Dockerfile
<code>
$ cd /data/mnist

$ vim mnist-cpu.Dockerfile
$ vim mnist-gpu.Dockerfile
</code>

### mnist-cpu.Dockerfile
Mnist cpu 的mnist-cpu.Dockerfile如下：
<code>
From uhub.ucloud.cn/uaishare/cpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0
ADD ././code/ /data/
</code>
Docker镜像build的时候会基于uhub.ucloud.cn/uaishare/cpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_tensorflow-1.1.0:v1.0，tensorflow 1.1的CPU基础镜像，然后将code/下面的代码拷贝到docker镜像的/data/目录下 

#### Build mnist-cpu 镜像
我们使用docker build命令来创建cpu镜像
<code>
$ sudo docker build -t tf-mnist-train-cpu:uaitrain -f mnist-cpu.Dockerfile .
</code>
这样可以生成一个镜像名为 **tf-mnist-train-cpu:uaitrain** 的CPU Mnist训练镜像。

### mnist-gpu.Dockerfile
Mnist cpu 的mnist-gpu.Dockerfile如下：
<code>
From uhub.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0

ADD ././code/ /data/
</code>
Docker镜像build的时候会基于uhub.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_tensorflow-1.1.0:v1.0，tensorflow 1.1的GPU基础镜像，然后将code/下面的代码拷贝到docker镜像的/data/目录下 

#### Build mnist-gpu 镜像
我们使用docker build命令来创建gpu镜像
<code>
$ sudo docker build -t uhub.service.ucloud.cn/uai_demo/tf-mnist-train:uaitrain -f mnist-gpu.Dockerfile .
</code>
这样可以生成一个镜像名为 **uhub.service.ucloud.cn/uai_demo/tf-mnist-train:uaitrain** 的GPU Mnist训练镜像。

