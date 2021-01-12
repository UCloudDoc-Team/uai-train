

# 在线服务
训练后的模型可以用于接收输入并进行推理(表情分类）。本例中模型接收图片作为输入，并返回图片上识别出的表情类别为结果。

我们使用Docker进行代码和模型的封装,你可以下载本文提供的镜像，也可以自己生成镜像。

## 下载镜像
你可以跳过下面的“自己生成镜像”步骤。

下载镜像：
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/slim_infer:latest
</code>
你需要将该镜像重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/slim_infer:latest

## 自己生成镜像

### 文件准备
我们使用Docker进行代码和模型的封装，需要准备如下数据文件：
<code>
|_ slim/

|_ sliminfer.py
|_ checkpoint_dir/
   |_ checkpoint 
   |_ info.json
   |_ labels.txt
   |_ model.ckpt...
|_ slim.conf
|_ slim_infer.Dockerfile
|_ sliminfer.py
</code>
其中checkpoint_dir保存了[UAI-Train案例](uai-train/cases/slim/train)中得到模型文件以及[tftrecord](uai-train/cases/slim/tfrecord)中得到的info.json和labels.txt。

**slim.conf介绍**
<code>
{
        "http_server" : {

​                "exec" : {
​                        "main_class": "slimModel",
​                        "main_file": "sliminfer"
​                },
​                "tensorflow" : {
​                        "model_dir" : "/data/output"
​                }
​        },
​       "infor" : {
​          "preprocessing_name" : "None",
​          "model_name" : "vgg_19",
​          "eval_image_size" : "None"
​        }
}
</code>

  * 其中http\_server.exec.main\_file定义了入口python模块:sliminfer.py（注：这边需要将.py 删除，因为django会以模块的形式import sliminfer）
  * 其中http\_server.exec.main\_class定义了推理服务的对象类：slimModel 
  * 其中http\_server.tensorflow.model\_dir定义了模型的路径 
  * 其中infor.preprocessing\_name给出了图像预处理方法（这里应该与模型训练时输入的preprocessing_name参数保持一致）
  * 其中infor.model\_name给出了模型名称（这里应该与模型训练时输入的model\_name参数保持一致）
  * 其中infor.eval\_image\_size给出了图片的resize大小（这里应该与模型训练时输入的train\_image\_size参数保持一致）

**slim\_infer.Dockerfile介绍**
<code>
FROM uhub.service.ucloud.cn/uaishare/cpu_uaiservice_ubuntu-16.04_python-2.7.6_tensorflow-1.6.0:v1.2

EXPOSE 8080
ADD ./slim/. /ai-ucloud-client-django/.
ADD ./sliminfer.py /ai-ucloud-client-django/sliminfer.py
ADD ./slim.conf  /ai-ucloud-client-django/conf.json
ADD ./checkpoint_dir/ /data/output/
ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi
</code>
  * 其中定义了使用的基础镜像uhub.service.ucloud.cn/uaishare/cpu\_uaiservice\_ubuntu-16.04\_python-2.7.6\_tensorflow-1.6.0:v1.2
  * export 8080端口
  * 将当前目录下code/下的文件复制到/ai-ucloud-client-django/
  * 将slim.conf 放入/ai-ucloud-client-django/conf.json
  * 指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
  * 启动http server

### 生成镜像
准备好以上文件之后，我们可以通过slim\_infer.Dockerfile生成Docker镜像。
<code>
sudo docker build -t uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/slim_infer -f slim_infer.Dockerfile .
</code>

## 本地测试
得到镜像之后，我们可以在本地进行测试。
<code>
sudo docker run -it -p 8080:8080 uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/slim_infer
</code>
我们使用fer2013数据集中的图片进行测试：
<code>
curl -X POST http://localhost:8080/service -T /data/fer/pic/angry/00000_1.jpg
</code>
命令行中会输出该图像的表情类别。

## UAI-Inference平台测试
可以查看部署在线服务的[具体操作步骤](uai-inference/tutorial/tf-mnist/gpu-inference)。
当部署完毕之后，我们可以在详细页面获取在线服务的URL地址。

我们可以通过如下命令来测试，这里的URL即为这个在线服务的URL地址：
<code>
curl -X POST http://<URL>/service -T /data/fer/pic/angry/00000_1.jpg
</code>
命令行中会输出该图像的表情类别。

