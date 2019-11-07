

# 模型训练
我们可以在本地或UAI-Train平台上进行模型训练。

## 数据集准备
如果在本地进行模型训练，则不需要提前准备数据。

如果在UAI-Train平台进行模型训练，则需要提前把数据上传到你准备使用的UCloud数据存储平台--UFile或者UFS上。

如果在进行平台训练之前，你已经在本地进行了模型训练，则可以在文件夹/data/data中得到cifar10数据集。
如果你没有在本地进行模型训练，可以通过如下命令在本地/data/data得到cifar10数据集：
<code>
sudo docker run -it -v /data/data:/data/data  uhub.service.ucloud.cn/uai-demo/cifar_cpu_train_simple:latest /bin/bash -c "python /data/data/download.py"
</code>
其中download.py存放在本地/data/data文件夹下，download.py的代码如下：
<code>
import cifar10

cifar10.maybe_download_and_extract()
</code>
如此我们可以在文件夹/data/data中得到cifar10数据集，将该数据集上传到你准备使用的UCloud数据存储平台--UFile或者UFS上。

### 上传数据

如果要将数据上传到UFile上，可以通过UFile操作工具进行数据的上传,UFile操作工具的下载参考[[ai:uai-train:set-up:tf-mnist:train|]]

上传命令（这里假设cifar10数据集放置在本地/data/data中，要上传到UFile的 uai-demo bucket的cifar\_simple/train/ 中，在你自己实验时请替换uai-demo bucket为你自己的UFIle bucket）：
<code>
./filemgr-linux64 --action mput --bucket uai-demo --dir /data/data/  --prefix cifar_simple/train/
</code>

## 本地训练
运行如下命令进行模型训练，如果/data/data文件夹中没有cifar数据集，该命令会把cifar10数据集下载到/data/data文件夹中，并将得到的模型文件保存在/data/output中。
<code>
sudo docker run -it -v /data/data:/data/data -v data/output:/data/output uhub.service.ucloud.cn/uai-demo/cifar_cpu_train_simple:latest /bin/bash -c "python /data/code/cifar10_train.py"
</code>

## 平台训练
在UAI-Train平台上进行模型训练的具体步骤可以参考[[ai:uai-train:set-up:tf-mnist:train|]]

首先填写训练任务名称等基本信息：
公私钥信息获取请参考：[[ai:uai-train:basic:key|]]
![](/ai/uai-train/images/case/cifar/a.png)

  * 选择我们上传的cifar\_gpu\_train\_simple:latest镜像；
  * 填写**数据集准备**步骤中设置的数据路径；
  * 设置结果输出路径
  * 填写训练启动命令

![](/ai/uai-train/images/case/cifar/h.png)

训练启动命令为：
<code>
/data/code/cifar10_train.py
</code>

训练结束后，可以在设置的结果输出路径查看输出的模型结果。

