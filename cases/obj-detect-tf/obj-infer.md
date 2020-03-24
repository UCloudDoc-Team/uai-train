

# 在线服务
训练后的模型可以用于接收输入并进行推理(物体识别）。本例中模型接收图片作为输入，并返回图片上识别出的物体（如宠物猫种类）和在图片上的位置为结果。

使用在线推理服务需将代码和模型打包为镜像。在线服务器从镜像中获得代码和模型并分别执行。在uhub共享镜像库我们提供了开源的Docker镜像可以进行在线推理服务：uhub.ucloud.cn/uai\_demo/tf-object-detect-infer-cpu:latest， (UCloud云主机可以通过uhub.service.ucloud.cn/uai\_demo/tf-object-detect-infer-cpu:latest 下载）。

## 启动推理服务

  - 获取uhub.ucloud.cn/uai\_demo/tf-object-detect-infer-cpu:latest镜像，并重新docker tag成你自己uhub 镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/tf-object-detect-infer-cpu:latest， 并提交至uhub。
  - 进入UCloud控制台，创建新的在线服务
  - 选取弹性服务，设置服务名称为：object-detect，选取8核8G机型，点击确定
  - 进入该服务条目，点击部署，选择镜像库中的：tf-object-detect-infer-cpu:latest，点击确定
  - 等待部署完毕，点击开启。此时在线服务已经开启

启动在线服务后，可以通过http访问进行测试。查看该在线服务管理页面-基本信息-服务URL地址。点击右侧重叠小方块图标复制访问地址。在本地主机的控制台中，转至有测试图片（例如：Persian_testcase.jpg）的路径。用curl工具模块进行测试：

<code>
curl -X POST http://[将访问地址黏贴在此]/service -T Persian_testcase.jpg
</code>

在控制台查看返回结果。可以多次测试检查准确度，并相应调整数据集或训练轮数。较大的训练数据集、较大和较清晰的图片以及较大的训练轮数往往能提高准确度。

**如果使用自定义图片集训练模型并打包推理镜像**，[参阅](uai-train/cases/obj-detect-tf/obj-packing)

