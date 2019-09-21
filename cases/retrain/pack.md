{{indexmenu_n>5}}

# 打包镜像

训练获得模型文件后，需与代码一同打包为镜像，通过镜像启动在线推理服务（关于在线推理服务的代码结构参阅[[ai:uai-train:set-up:tf-mnist:coding|]]）。下载Retrain推理代码包：

[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/retrain|Retrain - Tensorflow]]

并将路径及其所有子路径和文件保存在本地（此处默认保存在/data/目录下）。将frozen\_inference\_graph.pb文件和label\_map.txt（见：[[ai:uai-train:cases:retrain:train|模型训练]]）保存在其子路径：/data/retrain/code/checkpoint_dir/中。本地路径中保存的所有文件为：

<code>
/data/retrain/[路径]
/data/retrain/retrain-detect-cpu.Dockerfile
/data/retrain/retrain-detect.conf
/data/retrain/code/[路径]
/data/retrain/code/retrain_inference.py
/data/retrain/code/retrain_conf.py
/data/retrain/code/checkpoint_dir/[路径]
/data/retrain/code/checkpoint_dir/frozen_inference_graph.pb
/data/retrain/code/checkpoint_dir/label_map.pbtxt
</code>

其中/data/retrain/为根目录，其下保存了Dockerfile和conf文件用于打包镜像，以及code子目录。code中保存了代码（共计2个.py文件）和checkpoint\_dir子目录。将之前生成的pb模型文件和label\_map.pbtxt物体类别标签文件放在/data/object-detect/code/checkpoint\_dir/目录下。文件已准备好打包。

转至根目录/data/retrain/，运行命令打包镜像，标签为：uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/retrain-detect-infer:test，引用容器文件：retrain-detect-cpu.Dockerfile：

<code>
sudo docker build -t uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/retrain-detect-infer:test -f retrain-detect-cpu.Dockerfile .
</code>

请注意命令的最后有参数为单个英文句号“.”。等待打包完毕。完成后，获得镜像：uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/retrain-detect-infer:test。在本地主机登录UHub容器管理：

<code>
sudo docker login uhub.ucloud.cn
</code>

分别输入UCloud管理账号和密码，等待login success指示登录完毕。输入命令将打包好的镜像上传至Uhub镜像库：

<code>
sudo docker push uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/retrain-detect-infer:test
</code>

等待上传完毕。镜像已经上传至镜像库，参阅[[ai:uai-train:cases:retrain:infer|启动在线服务]]使用该镜像进行推理。

更多关于镜像打包的信息参阅[[ai:uai-train:set-up:tf-mnist:self-pack|使用自定义镜像打包]]

