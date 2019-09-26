{{indexmenu_n>4}}
# 模型训练
我们使用Tensorflow\_hub提供的重训练Python脚本训练模型（参阅：[[https://github.com/tensorflow/hub/blob/master/examples/image_retraining/retrain.py|Tensorflow_hub - retrain.py]]）。我们已经将脚本打包为可在UAI-Train上运行的容器镜像，并保存在镜像库中：

	uhub.ucloud.cn/uai_demo/retrain-train-v2:test

将准备好的图片集和模型文件上传至UFile准备进行重训练。根据上一节中的保存路径，在UFile中保存在统一根目录下。你可以自定义根路径，并记录此路径用于启动训练。此处我们以“uai/retrain/data”为根路径：

<code>
|_ uai/retrain/data

|  |_ pets
|  |  |_ Abyssinian
|  |  |  |_ Abyssinian_01.jpg
|  |  |  |_ Abyssinian_02.jpg
|  |  |_ Persian
|  |  |  |_ Persian_01.jpg
|  |  |  |_ Persian_02.jpg
|  |_ checkpoint_dir
|  |  |_ assets
|  |  |_ variables
|  |  |_ saved_model.pb
|  |  |_ tfhub_module.pb
</code>

文件准备好后，进入UAI-Train控制台。创建训练任务时，请确认数据输入源为保存数据的UFile根目录（此处为uai/retrain/data/，即数据准备一节中记录的数据保存路径），并确认模型、数据和配置文件均在此根目录下。记录数据输出源（此处为uai/retrain/output/），用于之后的在线服务。

  - 获取uhub.ucloud.cn/uai_demo/retrain-train-v2:test镜像，并重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/retrain-train-v2:test， 并提交至uhub。
  - 进入UAI-Train控制台，创建新训练任务。

	[[https://console.ucloud.cn/uaitrain/manage|UCloud - UAI训练]]

  - 填写以下信息：
    *   训练任务名称：retrain-train
    *   节点类型：单点式单卡
    *   公私钥：你的UCloud账号公私钥
    *   代码镜像路径：retrain-train-v2:test
    *   数据输入源：UFile：<YOUR\_UFILE\_PATH>/uai/retrain/data/
    *   数据输出源：UFile：<YOUR\_UFILE\_PATH>/uai/retrain/output/

  - 训练启动命令：

<code>
/data/retrain_v2.py --image_dir /data/data/pets --output_graph /data/output/frozen_inference_graph.pb --output_labels /data/output/label_map.txt --tfhub_module /data/data/checkpoint_dir
</code>

  - 点击确定，等待训练完成。

本重训练脚本内已实现了导出模型的功能，因此训练后的模型数据可直接用于识别推理，无需自行导出。因此模型数据经训练后所有参数即固定。如果模型准确度不理想，可在训练启动命令后加入参数：

<code>
--how_many_training_steps <轮数〉
</code>

以增加训练轮数，提高准确度，默认为4000轮。请注意轮数更大并不一定意味着模型预测更准确。

你也可以自行打包训练代码和镜像，参阅[[https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/retrain|Github - uai-sdk - retraining]]。