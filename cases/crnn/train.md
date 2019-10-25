

# 模型训练
我们使用Docker镜像将模型训练代码封装起来，再上传至UAI-Train平台进行训练。
如果你不想自己生成镜像，我们提供了封装好的Docker镜像，你可以通过下列命令在云主机上进行下载：
<code>
gpu镜像：
sudo docker pull uhub.service.ucloud.cn/uai_demo/ocr_gpu_train:v1.0

cpu镜像：
sudo docker pull uhub.service.ucloud.cn/uai_demo/ocr_cpu_train:v1.0
</code>
如果你使用了提供的镜像，则需要将下载的镜像重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/ocr\_cpu\_train:v1.0，并提交至uhub。

如果你准备自己生成镜像，可以通过下面的Dockerfile文件来完成。

## 生成Docker镜像
首先我们将训练所需数据和训练代码按照如下文件结构进行放置：

  * train\_feature.tfrecords、test\_feature.tfrecords、validation\_feature.tfrecords是我们在[[ai:uai-train:cases:crnn:tfrecords|]]中生成的文件；
  * char\_dict.json和ord\_map.json需要放置在data/data/文件夹下；
  * code中放置了相应的训练代码；

<code>
|_ data/

 |_ train_feature.tfrecords
 |_ test_feature.tfrecords
     |_ validation_feature.tfrecords
     |_ char_dict.json
     |_ ord_map.json
      |_ code/
     |_ crnnmodel/
     |_ data_provoder/
     |_ global_configuration/
     |_ local_utils/
     |_ tools/
      |_ output/
      |_ ocr-cpu.Dockerfile 
      |_ ocr-gpu.Dockerfile
    </code>

### Dockerfile介绍
我们可以生成gpu或者cpu的Docker镜像，下面以cpu的Docker镜像为例进行介绍。

我们以uhub.service.ucloud.cn/uaishare/cpu\_uaitrain\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.3.0:v1.0作为基础镜像，将code文件夹复制到了镜像中的/data/code文件夹。
<code>
From uhub.service.ucloud.cn/uaishare/cpu_uaitrain_ubuntu-16.04_python-3.6.2_tensorflow-1.3.0:v1.0

ADD /data/code /data/code
</code>

### 生成镜像
通过以下命令生成名为uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/ocr\_cpu\_train:v1.0的镜像.
<code>
sudo docker build -t uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/ocr_cpu_train:v1.0 -f ocr-cpu.Dockerfile .
</code>
我们将得到的镜像上传到UHub中以备训练时调用：
<code>
sudo docker push uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/ocr_cpu_train:v1.0 
</code>

## 训练
在生成镜像之前，我们可以在/data/code/global\_configuration/config.py中修改模型训练的相关参数，比如EPOCHS、LEARNING_RATE等等。

我们可以先在本地进行训练，测试训练能否正常进行，再在UAI-Train平台上进行训练。

### 本地训练

我们需要把训练需要的数据文件映射到uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/ocr\_cpu\_train:v1.0。
<code>
sudo docker run -it -v /data/data:/data/data /data/output:/data/output  uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/ocr_cpu_train:v1.0  /bin/bash -c "/usr/bin/python /data/code/tools/train_shadownet.py"
</code>
我们可以在/data/output中查看相应的训练输出。

### 平台训练
**上传训练数据**

我们需要将/data/data下的文件上传到UFile或者UFS中，在[[ai:uai-train:cases:crnn:tfrecords|]]中我们已经介绍了UFile平台的数据上传方法。

**执行训练命令**

在UAI-Train平台上训练的具体操作步骤可以参考[[ai:uai-train:set-up:tf-mnist:train|]]

训练的执行命令为：
<code>
/data/code/tools/train_shadownet.py 
</code>
我们可以在设置的输出路径中查看模型训练结果。


