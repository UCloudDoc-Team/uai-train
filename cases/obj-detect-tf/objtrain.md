{{indexmenu_n>5}}

# 模型训练
转换后的数据将被用于训练模型，以提升模型进行物体识别的能力。

我们提供了可以在AI Train平台上训练目标检测算法的基础镜像：uhub.ucloud.cn/uai\_demo/tf-objdetect-train-gpu-tf16:latest，可以直接用该镜像进行训练（如果已申请UCloud云主机，可以通过uhub.service.ucloud.cn/uai\_demo/tf-objdetect-train-gpu-tf16:latest下载该镜像）

## 训练前准备
我们基于faster\_rcnn\_resnet初始模型，用UFile中已生成的record数据训练识别模型。复制以下链接至浏览器下载初始模型：

storage.googleapis.com/download.tensorflow.org/models/object\_detection/faster\_rcnn\_resnet101\_coco\_11\_06\_2017.tar.gz

此模型为faster\_rcnn\_resnet101，针对目标识别进行过预训练，因此用于初始模型效率较高。将解压后的模型文件：

	model.ckpt.data-00000-of-00001; 
	model.ckpt.meta; 
	model.ckpt.index

放入UFile中tf-record文件的同一路径下，本例中为: uai/object-prep-output/

模型训练需要配置文件设定配置。下载所用配置文件：

[[https://github.com/tensorflow/models/blob/master/research/object_detection/samples/configs/faster_rcnn_resnet101_pets.config|宠物模型训练配置文件, faster_rcnn_resnet101.config]]

重命名为faster\_rcnn\_resnet101.config并保存。此配置文件中有几处需要修改以适配我们的文件命名，否则无法正确读取图片数据：

<code>
line 122:
input_path: "PATH_TO_BE_CONFIGURED/pet_train.record-?????-of-00010" -> input_path: "PATH_TO_BE_CONFIGURED/obj_train.record*"

line 124:
label_map_path: "PATH_TO_BE_CONFIGURED/pet_label_map.pbtxt" -> label_map_path: "PATH_TO_BE_CONFIGURED/label_map.pbtxt"

line 134:
input_path: "PATH_TO_BE_CONFIGURED/obj_val.record-?????-of-00010" -> input_path: "PATH_TO_BE_CONFIGURED/obj_val.record*"

line 136:
label_map_path: "PATH_TO_BE_CONFIGURED/pet_label_map.pbtxt" -> label_map_path: "PATH_TO_BE_CONFIGURED/label_map.pbtxt"
</code>

如果使用自定义图片数据集，需将第9行的右边常数替换为自定义图片集中物体种类的数量：

<code>
num_classes = 37 -> 自定义种类数量
</code>

物体识别的训练轮次默认为200000轮，足够达到较准确的预测结果。你也可以自行调整训练轮数比对训练效果，可将第112行自行修改：

<code>
num_steps = 200000 -> 自定义训练轮数
</code>

调整好参数后，将此文件连同类别标签文件label_map.pbtxt（见：[[ai:uai-train:cases:obj-detect-tf:data|]]）保存在UFile同一路径下。此时UFile中已准备好的训练数据有：

<code>
uai/object-prep-output/[路径]

uai/object-prep-output/label_map.pbtxt
uai/object-prep-output/obj_train.record-00000-of-00010
uai/object-prep-output/obj_train.record-00001-of-00010
...
uai/object-prep-output/obj_train.record-00009-of-00010
uai/object-prep-output/obj_val.record-00000-of-00010
uai/object-prep-output/obj_val.record-00001-of-00010
...
uai/object-prep-output/obj_val.record-00009-of-00010
uai/object-prep-output/model.ckpt.data-00000-of-00001
uai/object-prep-output/model.ckpt.meta; 
uai/object-prep-output/model.ckpt.index
uai/object-prep-output/faster_rcnn_resnet101.config
</code>

检查是否已包含tf-record文件（obj\_train.record\*, obj\_val.record\*），类别标签文件（label\_map.pbtxt），初始模型文件（model.ckpt.*），以及模型配置文件（faster\_rcnn\_resnet101.config），并完全与以上命名一致，否则训练任务无法启动。

## 使用AI Train平台训练
数据和模型已准备好进行训练。创建训练任务时，请确认数据输入源为保存数据的UFile根目录（此处为uai/object-prep-output/，即数据转换一节中记录的数据输出源），并确认模型、数据和配置文件均在此根目录下。记录数据输出源（此处为uai/object-train-output/）以进行在线服务。

  - 获取uhub.ucloud.cn/uai\_demo/tf-objdetect-train-gpu-tf16:latest镜像，并重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/tf-objdetect-train-gpu-tf16:latest， 并提交至uhub。
  - 进入UAI-Train控制台，创建新训练任务。

	[[https://console.ucloud.cn/uaitrain/manage|UCloud - UAI训练]]

  - 填写以下信息：
    *   训练任务名称：object-detect-train
    *   节点类型：单点式单卡
    *   公私钥：你的UCloud账号公私钥
    *   代码镜像路径：tf-objdetect-train-gpu-tf16:latest
    *   数据输入源：UFile：<YOUR\_UFILE\_PATH>/uai/object-prep-output/
    *   数据输出源：UFile：<YOUR\_UFILE\_PATH>/uai/object-train-output/
  - 训练启动命令：/data/object\_detection/train.py \--pipeline\_config\_path /data/data/faster\_rcnn\_resnet101.config \--train_dir /data/output

  - 点击确定，等待训练完成。

训练完成后，可在UFile输出目录（此处为uai/object-train-output/）找到训练完毕的模型：
<code>
uai/object-train-output/[路径]

uai/object-train-output/model.ckpt-200000.data-00000-of-00001
uai/object-train-output/model.ckpt-200000.meta
uai/object-train-output/model.ckpt-200000.index
uai/object-train-output/pipeline.config
uai/object-train-output/checkpoint
...
</code>

这些为训练后的模型文件。更多关于在线训练的信息参阅：

[[ai:uai-train:tutorial:tf-mnist:train|创建在线训练指南]]

