{{indexmenu_n>6}}

# 在线推理服务

上一节中训练得到的模型可直接用于推理。我们已对resnet\\\_v1\\\_101\\\_224识别模型，用本例中的宠物图片数据进行重训练，并打包了在线推理镜像在UHub镜像库中：

    uhub.ucloud.cn/uai_demo/retrain-resnet-pets-infer-cpu:latest

### 启动推理服务

1.  获取uhub.ucloud.cn/uai\\\_demo/retrain-resnet-pets-infer-cpu:latest镜像，并重新docker
    tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/\<YOUR\\\_UHUB\\\_REGISTRY\>/retrain-resnet-pets-infer-cpu:latest，
    并提交至uhub。
2.  进入UCloud控制台，创建新的在线服务：[console](/ai/uai-inference/use/new/console)
3.  选取弹性服务，设置服务名称为：retrain-detect-pets，选取8核8G机型，点击确定
4.  进入该服务条目，点击部署，选择镜像库中的：retrain-resnet-pets-infer:latest，点击确定
5.  等待部署完毕，点击开启。此时在线服务已经开启

启动在线服务后，可以通过http访问进行测试。查看该在线服务管理页面-基本信息-服务URL地址。点击右侧重叠小方块图标复制访问地址。在本地主机的控制台中，转至有测试图片（例如：Persian\_testcase.jpg）的路径。用curl工具模块进行测试：

    curl -X POST http://[将访问地址黏贴在此]/service -T Persian_testcase.jpg

在控制台查看返回结果。可以多次测试检查准确度，并相应调整数据集或训练轮数。较大的训练数据集、较大和较清晰的图片以及较大的训练轮数往往能提高准确度。

如果希望测试其他模型以及自定义数据集训练好的模型，参照[打包自定义镜像](/ai/uai-train/cases/retrain/pack)打包镜像，再回到本节进行推理。
