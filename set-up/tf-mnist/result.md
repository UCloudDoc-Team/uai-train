

# 获取结果
训练完毕后系统会自动将/data/output/下的数据上传到Ufile指定的输出中，本案例的输出地址为：[[http://uai-demo.ufile.ucloud.com.cn/mnist/output/]]，你可以在训练任务**详细信息**->**概览**页面->**执行信息** 中找到相关信息。

## 下载训练结果
### UFile操作界面下载
我们可以在[[https://console.ucloud.cn/ufile/ufile/manage/normal]] 下载输出镜像，根据mnist/output/这个prefix来查询：
{{:ai:uai-train:tutorial:tf-mnist:download.png?|}}

然后在界面直接下载

### 命令行下载
我们同样可以使用filemgr-linux64 来下载数据：
<code>
$ cd ~/filemgr-linux64

$ ./filemgr-linux64 --action download --bucket uai-demo --key mnist/output/checkpoint --file /data/mnist/gpu-output/checkpoint
$ ./filemgr-linux64 --action download --bucket uai-demo --key mnist/output/model.ckpt.data-00000-of-00001 --file /data/mnist/gpu-output/model.ckpt.data-00000-of-00001
$ ./filemgr-linux64 --action download --bucket uai-demo --key mnist/output/model.ckpt.index --file /data/mnist/gpu-output/model.ckpt.index
$ ./filemgr-linux64 --action download --bucket uai-demo --key mnist/output/model.ckpt.meta --file /data/mnist/gpu-output/model.ckpt.meta
</code>

此时我们可以在/data/mnist/gpu-output/找到训练好的模型。之后我们可以将其部署在AI Inference平台


