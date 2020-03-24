

# 自定义打包镜像
在安装自定义软件包之前请确认已经执行了pack操作，如何执行请参见[](uai-train/guide/tensorflow/packing) 

## Step0: 获取dockerfile
在完成pack操作后，系统会生成两个Dockerfile:

  * **uaitrain.Dockerfile** 生成GPU镜像所使用的Dockerfile
  * **uaitrain-cpu.Dockerfile** 生成CPU镜像所使用的Dockerfile

## Step1: 修改dockerfile
### 修改GPU Dockerfile
我们打开uaitrain.Dockerfile 可以看到如下内容：
<code>
From uhub.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0

ADD ././code/ /data/
</code>

我们可以通过修改uaitrain.Dockerfile的方式来修改打包内容，例如我们要增加安装NLTK软件包，我们可以将dockerfile修改成：
<code>
From uhub.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0

RUN pip install -U nltk -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
ADD ././code/ /data/
</code>

我们在将/mnist\_summary\_1.1/code/下面的内容拷贝至 Docker容器的/data/目录下之前执行了
<code>
RUN pip install -U nltk -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
</code>
用以安装了nltk软件包 

### 修改CPU Dockerfile
我们打开uaitrain-cpu.Dockerfile 可以看到如下内容：
<code>
From uhub.ucloud.cn/uaishare/cpu_uaitrain_ubuntu-14.04_python-2.7.6_tensorflow-1.1.0:v1.0

ADD ././code/ /data/
</code>

使用和修改GPU Dockerfile相同的方法修改uaitrain-cpu.Dockerfile即可

**注：详细的dockerfile使用规范请查询Docker官方文档**

## Step2: 重新打包Docker镜像
修改完dockerfile之后我们就可以重新打包Docker镜像了

### 打包GPU 镜像
我们可以执行以下指令来打包GPU镜像：
<code>
sudo docker build -t uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/<YOUR_NEW_IMAGENAME>:<YOUR_NEW_IMG_TAG> -f uaitrain.Dockerfile .
</code>
执行完成后可以获得新的镜像：
<code>
uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/<YOUR_NEW_IMAGENAME>:<YOUR_NEW_IMG_TAG>
</code>

**案例**
<code>
sudo docker build -t uhub.ucloud.cn/demo/test:uaitrain -f uaitrain.Dockerfile .
</code>

### 打包CPU 镜像
由于CPU镜像仅用户本地测试，因此打包也比较简单，可以执行以下指令来打包CPU镜像：
<code>
sudo docker build -t <YOUR_NEW_IMAGENAME>-cpu:<YOUR_NEW_IMG_TAG> -f uaitrain-cpu.Dockerfile .
</code>
执行完成后可以获得新的镜像：
<code>
<YOUR_NEW_IMAGENAME>-cpu:<YOUR_NEW_IMG_TAG>
</code>

**案例**
<code>
sudo docker build -t test-cpu:uaitrain -f uaitrain-cpu.Dockerfile .
</code>

## Step3: 本地测试Docker镜像
我们可以使用[](uai-train/guide/tensorflow/packing) 《Step4: 输出说明》中的类似案例运行本地测试。
测试时需要讲docker镜像的名字做修改：

### GPU测试
<code>
sudo nvidia-docker run -it -v /data/test/data/:/data/data -v /data/test/output/:/data/output uhub.ucloud.cn/<YOUR_UHUB_REFDISTRY>/<YOUR_NEW_IMAGENAME>:<YOUR_NEW_IMG_TAG> /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
</code>

**GPU测试(mnist\_summary例子):**
<code>
sudo nvidia-docker run -it -v /data/test/data/:/data/data -v /data/test/output/:/data/output uhub.ucloud.cn/demo/test:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
</code>

注：案例使用的uhub.ucloud.cn/demo/ 是虚拟registry，不可docker push

### CPU测试
<code>
sudo docker run -it -v /data/test/data/:/data/data -v /data/test/output/:/data/output <YOUR_NEW_IMAGENAME>-cpu:<YOUR_NEW_IMG_TAG> /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
</code>

**CPU测试(mnist\_summary例子):**
<code>
sudo docker run -it -v /data/test/data/:/data/data -v /data/test/output/:/data/output test-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
</code>
## Step4: 登录UHub公共镜像库
上传镜像之前，需先登录Uhub，且确保控制台的Uhub创建过对应镜像库。
<code>
sudo docker login uhub.ucloud.cn
</code>
登录信息为控制台登录的对应账号密码。

## Step5: 上传Docker镜像
UAI-Train平台目前仅接受GPU镜像训练，因此在上传镜像时我们只需要上传GPU的Docker镜像。
<code>
sudo docker push uhub.ucloud.cn/<YOUR_UHUB_REFDISTRY>/<YOUR_NEW_IMAGENAME>:<YOUR_NEW_IMG_TAG> 
</code>
之后就可以使用该镜像了

