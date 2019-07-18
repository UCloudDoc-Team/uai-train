{{indexmenu_n>6}}

====== 打包镜像 ======
在线推理服务基于镜像。本节介绍如何将训练好的模型文件与代码一并打包为镜像。
==== 转换训练生成模型的格式 ====
如果通过在线训练获得了识别模型，可以自行打包为镜像并上传至Uhub以启动在线推理。转至UFile并查找模型训练一节中记录的模型输出源（本例中为uai/object-train-output/），检查训练后的模型文件（以训练200000轮的模型文件为例）：

<code>
uai/object-train-output/model.ckpt-200000.data-00000-of-00001
uai/object-train-output/model.ckpt-200000.meta
uai/object-train-output/model.ckpt-200000.index
uai/object-train-output/pipeline.config
uai/object-train-output/checkpoint
</code>

这些是训练后的模型文件，我们用这些文件导出推理使用的pb格式的模型文件。Uhub中保存了用于导出模型文件的镜像，我们继续用在线训练服务导出模型。创建训练服务时，将以上模型存储的根目录作为输入源（此处为uai/object-train-output），并记录模型导出输出源以进行镜像打包。

  * 获取uhub.ucloud.cn/uai_demo/object-detect-export-model:latest镜像，并重新docker tag成你自己uhub镜像库中的镜像，例如：uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/object-detect-export-model:latest， 并提交至uhub。
  * 进入UAI-Train控制台，创建新训练任务。

	[[https://console.ucloud.cn/uaitrain/manage|UAI - Train]]

  * 填写以下信息：
    - 训练任务名称：object-detect-export-model
    - 节点类型：单点式单卡
    - 公私钥：你的UCloud账号公私钥
    - 代码镜像路径：object-detect-export-model:latest
    - 数据输入源：UFile：<YOUR\_UFILE\_PATH>/uai/object-train-output/
    - 数据输出源：UFile：<YOUR\_UFILE\_PATH>/uai/object-export-output/
    - 训练启动命令：/data/object\_detection/export\_inference\_graph.py \--input\_type image\_tensor \--pipeline\_config\_path /data/data/pipeline.config \--trained\_checkpoint\_prefix /data/data/model.ckpt-200000 \--output\_directory /data/output/
    - 点击确定，等待模型导出完成。

导出完成后，转至UFile库中搜索输出源前缀（此处为uai/object-export-output/）：<YOUR\_UFILE\_PATH>/uai/object-export-output/，找到frozen\_inference\_graph.pb文件并下载，即为导出的模型。

如想在本地通过脚本导出模型文件，参阅：

[[https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/exporting_models.md|Object Detection - Exporting Models]]


==== 打包自定义模型在线服务镜像 ====

生成pb格式的模型文件后，需与代码一同打包为镜像，通过镜像启动在线推理服务（关于在线推理服务的代码结构参阅[[ai:uai-train:tutorial:tf-mnist:coding|]]）。下载Object Detection推理代码包：

[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/object-detect|Object Detection - Tensorflow]]

并将路径及其所有子路径和文件保存在本地（此处默认保存在/data/目录下）。将frozen\_inference\_graph.pb文件和label\_map.pbtxt（见：[[ai:uai-train:cases:obj-detect-tf:data|原始数据准备]]）保存在其子路径：/data/object-detect/code/checkpoint_dir/中。本地路径中保存的所有文件为：

<code>
/data/object-detect/[路径]
/data/object-detect/object-detect-cpu.Dockerfile
/data/object-detect/object-detect.conf
/data/object-detect/code/[路径]
/data/object-detect/code/object_detect_inference.py
...
/data/object-detect/code/string_int_label_map_pb2.py
/data/object-detect/code/checkpoint_dir/[路径]
/data/object-detect/code/checkpoint_dir/frozen_inference_graph.pb
/data/object-detect/code/checkpoint_dir/label_map.pbtxt
</code>

其中object-detect为根目录，其下保存了Dockerfile和conf文件用于打包镜像，以及code子目录。code中保存了代码（共计9个.py文件）和checkpoint\_dir子目录。将之前生成的pb模型文件和label\_map.pbtxt物体类别标签文件放在/data/object-detect/code/checkpoint\_dir/目录下。文件已准备好打包。

转至根目录/data/object-detect/，运行命令打包镜像，标签为：uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/object-detect-infer:test，引用容器文件：object-detect-cpu.Dockerfile：

<code>
sudo docker build -t uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/object-detect-infer:test -f object-detect-cpu.Dockerfile .
</code>

请注意命令的最后有参数为单个英文句号“.”。等待打包完毕。完成后，获得镜像：uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/object-detect-infer:test。在本地主机登录UHub容器管理：

<code>
sudo docker login uhub.ucloud.cn
</code>

分别输入UCloud管理账号和密码，等待login success指示登录完毕。输入命令将打包好的镜像上传至Uhub镜像库：

<code>
sudo docker push uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/object-detect-infer:test
</code>

等待上传完毕。镜像已经上传至镜像库，参阅[[ai:uai-train:case:obj-detect-tf:objinfer|启动在线推理服务]]使用该镜像进行推理。

更多关于镜像打包的信息参阅[[ai:uai-train:set-up:tf-mnist:self-pack|使用自定义镜像打包]]

