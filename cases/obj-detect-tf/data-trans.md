

注意：本节需在本地主机使用Docker模块，请参照[在本地安装工作环境](uai-train/set-up/tf-mnist/prepare)。

# 数据格式转换
收集以上数据后，我们就可以运行脚本生成tfrecord文件。tfrecord记录了图片和事实数据集，并可作为训练的输入数据。在uhub共享镜像库我们提供了开源的Docker镜像进行转换：
uhub.ucloud.cn/uai\_demo/object-detect-preprocess:latest 
UCloud云主机可以通过 uhub.service.ucloud.cn/uai_demo/object-detect-preprocess:latest 下载。

## 上传数据集

  - 登录UCloud控制台，创建[US3存储空间](https://console.ucloud.cn/ufile/ufile)，将以上数据保存在US3库中，注意设置US3路径前缀。

例如以如下目录设置前缀并上传记录的根目录中的文件，则US3根目录为uai/object-prep/，每个文件的前缀为此根目录加路径：
<code>

uai/object-prep/label_map.pbtxt
uai/object-prep/annotations/trainval.txt
uai/object-prep/annotations/xmls/Abyssinian_01.xml
uai/object-prep/annotations/xmls/Abyssinian_02.xml
uai/object-prep/annotations/xmls/basset_hound_01.xml
uai/object-prep/images/Abyssinian_01.jpg
uai/object-prep/images/Abyssinian_02.jpg
uai/object-prep/images/basset_hound_01.jpg
</code>

例如，对于trainval.txt，则前缀为uai/object-prep/annotations。
## 使用UAI-Train平台实现数据转换
UAI-Train在线平台可以在US3平台上读取和写入数据，可运行Uhub中的代码镜像。我们用Uhub镜像库中的数据转换镜像进行数据转换。注意创建任务时数据输入源应为上传文件至US3时的US3根目录，例如之前所用的：uai/object-prep/。数据输出源不可与输入源相同，请记录数据输出源用于模型训练，此处为uai/object-prep-output/。

  - 获取uhub.ucloud.cn/uai\_demo/object-detect-preprocess:latest镜像，并重新docker tag成你自己uhub 镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/object-detect-preprocess:latest， 并提交至uhub。
  - 进入UAI-Train控制台，创建[新训练任务](https://console.ucloud.cn/uaitrain/manage)。

  - 填写以下信息：
    * 训练任务名称：object-detect-preprocess
    * 节点类型：单点式单卡
    * 代码镜像：object-detect-preprocess:latest
    * 公私钥：您的UCloud账号公私钥
    * 数据输入源：US3：<YOUR\_UFILE\_PATH>/uai/object-prep/
    * 数据输出源：US3：<YOUR\_UFILE\_PATH>/uai/object-prep-output/
    * 训练启动命令：create\_tf\_record.py

  - 点击确定，等待数据转换完成。

转换完毕后，tf-record将被保存在US3库的输出源（此处为uai/object-prep-output/）中。一共有20个记录文件：

<code>
uai/object-prep-output/[路径]

uai/object-prep-output/obj_train.record-00000-of-00010
uai/object-prep-output/obj_train.record-00001-of-00010
...
uai/object-prep-output/obj_train.record-00009-of-00010
uai/object-prep-output/obj_val.record-00000-of-00010
uai/object-prep-output/obj_val.record-00001-of-00010
...
uai/object-prep-output/obj_val.record-00009-of-00010
</code>

其中obj\_train.record和obj\_val.record分别用于训练中的训练阶段和评估阶段，分别占原始数据的70%和30%。预处理时图片被随机分配，因此每次训练后模型的参数可能略有差异，但不影响准确度。

更多关于在线训练的信息参阅[创建在线训练指南](uai-train/set-up/tf-mnist/train)

如希望在本地主机进行，可参考[Pet Model Training]([https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_pets.md)进行类型转换。

