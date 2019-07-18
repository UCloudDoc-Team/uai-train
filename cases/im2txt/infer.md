{{indexmenu_n>7}}

====== 在线服务 ======

训练好模型后，模型即可被用来对任意图片生成文字描述。1000000轮次的一轮训练即可使模型达到标准的语法；如果进行第二轮训练，则文字的语法和内容都可达到较理想的效果。一些测试结果表明第二轮训练到3000000轮可以达到最优效果，你也可以试用不同训练轮数的结果测试训练效果，参阅[[ai:uai-train:cases:im2txt:train|]]。我们的UHub镜像库中已经通过2000000轮训练获取了一个模型并打包为镜像：

<code>
uhub.ucloud.cn/uai_demo/im2txt-infer:latest
</code>

==== 启动推理服务 ====
  - 获取uhub.ucloud.cn/uai\_demo/im2txt-infer:latest镜像，并重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/im2txt-infer:latest， 并提交至uhub。
  - 进入UCloud控制台，创建新的在线服务：[[ai:uai-inference:use:new:console]]
  - 选取弹性服务，设置服务名称为：im2txt-captioning，选取1核1G机型，点击确定
  - 进入该服务条目，点击部署，选择镜像库中的：im2txt-infer:latest，点击确定
  - 等待部署完毕，点击开启。此时在线服务已经开启

启动在线服务后，可以通过http访问进行测试。查看该在线服务管理页面-基本信息-服务URL地址。点击右侧重叠小方块图标复制访问地址。在本地主机的控制台中，转至有测试图片（例如：boy-play-football.jpg）的路径。用curl工具模块进行测试：

<code>
curl -X POST http://[将访问地址黏贴在此]/service -T boy-play-football.jpg
</code>

在控制台查看返回结果。可以多次测试检查准确度，并相应调整数据集或训练轮数。较大的训练数据集、较大和较清晰的图片以及较大的训练轮数往往能提高准确度。

如果希望测试其他模型以及自定义数据集训练好的模型，参照[[ai:uai-train:cases:im2txt:pack|打包自定义镜像]]打包镜像，再回到本节进行推理。