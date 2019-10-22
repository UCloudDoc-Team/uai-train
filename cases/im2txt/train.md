{{indexmenu_n>5}}

# 模型训练
转换后的数据将被用于训练模型，以赋予模型给模型添加文字标注的能力。

我们提供了可以在AI Train平台上训练目标检测算法的基础镜像：uhub.ucloud.cn/uai\_demo/im2txt-train-gpu:test，可以直接用该镜像进行训练（如果已申请UCloud云主机，可以通过uhub.service.ucloud.cn/uai\_demo/im2txt-train-gpu:test下载该镜像）

将准备好的tfrecord文件和inception模型文件上传至UFile，并保留路径格式。将uai-sdk中的字典文件[[https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/train/im2txt/word_counts.txt|uai-sdk字典文件]]放在相同路径下。例如保存在uai/im2txt/data路径下：

	# uai/im2txt/data/
	  inception_v3.ckpt
	  train-00000-of-00256 train-00001-of-00256 ... train-00255-of-00256
	  test-00000-of-00008 test-00001-of-00008 ... test-00007-of-00008
	  val-00000-of-00004 val-00001-of-00004 ... val-00001-of-00004
	  word_counts.txt

共计270个文件。
# 使用AI Train平台训练
数据和模型已准备好进行训练。创建训练任务时，请确认数据输入源为保存数据的UFile根目录（此处为uai/im2txt/data），并确认模型、数据和字典文件均在此根目录下。

  - 获取uhub.ucloud.cn/uai\_demo/im2txt-train-gpu:test镜像，并重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REP>/im2txt-train-gpu:test， 并提交至uhub。
  - 进入UAI-Train控制台，创建新训练任务。

	[[https://console.ucloud.cn/uaitrain/manage|UCloud - UAI训练]]

  - 填写以下信息：
    *   训练任务名称：im2txt-train
    *   节点类型：单点式单卡
    *   公私钥：你的UCloud账号公私钥
    *   代码镜像路径：im2txt-train-gpu:test
    *   数据输入源：UFile：<YOUR\_UFILE\_PATH>/uai/im2txt/data/
    *   数据输出源：UFile：<YOUR\_UFILE\_PATH>/uai/im2txt/output/
  - 训练启动命令：

公私钥信息获取请参考：[[ai:uai-train:basic:key|]] 

<code>
/data/train.py --input_file_pattern=/data/data/train-?????-of-00256 --inception_checkpoint_file=/data/data/inception_v3.ckpt --train_dir=/data/output/ --number_of_steps=1000000
</code>

  - 点击确定，等待训练完成。

训练完成后，可在UFile输出目录（此处为uai/object-train-output/）找到训练完毕的模型：
<code>
uai/im2txt/output/[路径]
uai/im2txt/output/model.ckpt-1000000.data-00000-of-00001
uai/im2txt/output/model.ckpt-1000000.meta
uai/im2txt/output/model.ckpt-1000000.index
uai/im2txt/output/checkpoint
...
</code>

这些为训练后的模型文件。

# 第二轮训练
以上的训练足够使模型给出较理想的结果（基本符合语法规范、基本贴近图片内容）。但是如果进行更多的训练进行全参数微调，模型仍有提升空间。如果希望获得更好的训练结果，重复上方的训练过程。此时，将训练启动命令修改为：

<code>
/data/train.py --input_file_pattern=/data/data/train-?????-of-00256 --train_dir=/data/output/ --number_of_steps=3000000 --train_inception=True
</code>


注意其中训练步数参数被设置为3000000，训练inception参数被设置为True。第二轮训练将对整个模型的参数进行联动微调以更好的给出文字标注结果。注意本轮训练由于被训练的参数较多且轮数较大（第一轮为1000000轮，第二轮在其基础上训练额外2000000轮至3000000轮），耗时更久。训练结束后，可在UFile中获取新的模型文件：

<code>
uai/im2txt/output/[路径]

uai/im2txt/output/model.ckpt-3000000.data-00000-of-00001
uai/im2txt/output/model.ckpt-3000000.meta
uai/im2txt/output/model.ckpt-3000000.index
uai/im2txt/output/checkpoint
...
</code>

