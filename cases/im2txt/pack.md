{{indexmenu_n>6}}

# 打包镜像

训练获得模型文件后，需与代码一同打包为镜像，通过镜像启动在线推理服务（关于在线推理服务的代码结构参阅[[ai:uai-train:set-up:tf-mnist:coding|]]）。下载Im2txt推理代码包：

[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/im2txt|Im2txt - Tensorflow]]

并将路径及其所有子路径和文件保存在本地（此处默认保存在/data/目录下）。将训练后的模型文件和word\_counts.txt（见：[[ai:uai-train:cases:im2txt:train|模型训练]]）保存在其子路径：/data/im2txt/code/checkpoint_dir/中。本地路径中保存的所有文件为：

<code>
/data/im2txt/
|_ code
|  |_ checkpoint_dir
|  |  |_ checkpoint
|  |  |_ model.ckpt-3000000.data-00000-of-00001
|  |  |_ model.ckpt-3000000.meta
|  |  |_ model.ckpt-3000000.index
|  |  |_ word_counts.txt
|  |_ im2txt_inference.py
|  |_ im2txt_conf.py
|  |_ configuration.py
|  |_ inference_wrapper.py
|  |_ show_and_tell_model.py
|  |_ inference_utils
|  |_ ops
|_ im2txt.conf
|_ im2txt-infer-cpu.Dockerfile
</code>

其中/data/im2txt/为根目录，其下保存了Dockerfile和conf文件用于打包镜像，以及code子目录。code中保存了代码（共计5个.py文件和ops, inference\_utils两个模块包目录）和checkpoint\_dir子目录。将之前生成的模型文件（分别为model.ckpt-{轮数}.meta, model.ckpt-{轮数}.index, model.ckpt-{轮数}.data-00000-of-00001, checkpoint。本例中一轮训练结束为1000000轮，二轮训练结束为3000000轮）和word_counts.txt字典文件放在/data/im2txt/code/checkpoint\_dir/目录下。文件已准备好打包。

转至根目录/data/im2txt/，运行命令打包镜像，标签为：uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/im2txt-infer:test，引用容器文件：im2txt-infer-cpu.Dockerfile：

<code>
sudo docker build -t uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/im2txt-infer:test -f im2txt-infer-cpu.Dockerfile .
</code>

请注意命令的最后有参数为单个英文句号“.”。等待打包完毕。完成后，获得镜像：uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/im2txt-infer:test。在本地主机登录UHub容器管理：

<code>
sudo docker login uhub.ucloud.cn
</code>

分别输入UCloud管理账号和密码，等待login success指示登录完毕。输入命令将打包好的镜像上传至Uhub镜像库：

<code>
sudo docker push uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/im2txt-infer:test
</code>

等待上传完毕。镜像已经上传至镜像库，参阅[[ai:uai-train:cases:retrain:infer|启动在线服务]]使用该镜像进行推理。

更多关于镜像打包的信息参阅[[ai:uai-train:set-up:tf-mnist:self-pack|使用自定义镜像打包]]