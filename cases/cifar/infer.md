

# 在线服务
训练后的模型可以用于接收输入并进行推理(图像分类）。本例中模型接收图片作为输入，并返回图片上识别出的物体类别为结果。

我们使用Docker进行代码和模型的封装,你可以下载本文提供的镜像，也可以自己生成镜像。

## 下载镜像
你可以跳过下面的“自己生成镜像”步骤。

下载镜像：
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/cifar_infer_simple:latest
</code>
你需要将该镜像重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/cifar\_infer\_simple:latest

## 自己生成镜像

### 文件准备
我们使用Docker进行代码和模型的封装，需要准备如下数据文件：
<code>
|_ code/

|_ inference/
   |_ cifar_inference.py 
   |_ checkpoint_dir/
|_ cifar.conf
|_ cifar_infer.Dockerfile
</code>
其中checkpoint_dir保存了[UAI-Train案例](uai-train/cases/cifar/train)中得到模型文件。

#### cifar.conf介绍

<code>
{                                                                               
	"http_server" : {                                                                                              
		"exec" : {                                                                                  
			"main_class": "cifarModel",                                                                            
			"main_file": "cifar_infer"                                                                           
		},                                                                                                 
		"tensorflow" : {                                                                                        
			"model_dir" : "./checkpoint_dir"                                                                       
		}                                                                                           
	}                                                                                                       
} 
</code>

  * 其中exec.main\_file定义了入口python模块:cifar\_infer.py（注：这边需要将.py 删除，因为django会以模块的形式import cifar\_inference）
  * 其中exec.main\_class定义了推理服务的对象类：cifarModel 
  * 其中tensorflow.model\_dir定义了模型的相对路径 

#### cifar\_infer.Dockerfile介绍
<code>
FROM uhub.service.ucloud.cn/uaishare/cpu_uaiservice_ubuntu-14.04_python-2.7.6_tensorflow-1.4.0:v1.2
EXPOSE 8080

ADD ./inference/ /ai-ucloud-client-django/
ADD ./code/ /ai-ucloud-client-django/
ADD ./cifar.conf  /ai-ucloud-client-django/conf.json
ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi
</code>

  * 其中定义了使用的基础镜像uhub.service.ucloud.cn/uaishare/cpu\_uaiservice\_ubuntu-14.04\_python-2.7.6\_tensorflow-1.4.0:v1.2
  * export 8080端口
  * 将当前目录下code/下的文件复制到/ai-ucloud-client-django/
  * 将cifar.conf 放入/ai-ucloud-client-django/conf.json
  * 指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
  * 启动http server

#### cifar_inference.py介绍

cifar\_inference.py首先需要实现一个在线服务的类，该类继承了TFAiUcloudModel（TensorFlow 在线服务基类）. cifar\_inference.py实现了load\_model和execute两个函数。

**1. load\_model(self)**:

该函数加载了训练好的cnn模型。
<code>
def load_model(self):
   sess = tf.Session()
   x = tf.placeholder(dtype=tf.float32, shape=[1, 24, 24, 3], name='input')
   #inferece
   pred = tf.argmax(cifar10.inference(x),axis=1)
   #load model
   saver = tf.train.Saver()
   params_file = tf.train.latest_checkpoint(self.model_dir)
   saver.restore(sess=sess, save_path=params_file)
   #Register ops into self.output dict.So func execute() can get these ops
   self.output['sess'] = sess
   self.output['x'] = x
   self.output['y_'] = pred
</code>

**2. execute(self, data, batch\_size)** :

该函数从data获取batching的请求数据，并转化为numpy list；通过sess.run请求推理操作；将请求结果合并成ret（ret也是一个list，和data list是一一对应的关系）
<code>
def execute(self, data, batch_size):

sess = self.output['sess']
   x = self.output['x']
   y_ = self.output['y_']
   ret = []
   for i in range(batch_size):
​	'''
​	1 load data 
​	'''
​	image = Image.open(data[i])
​	image = cv2.cvtColor(np.asarray(image),cv2.COLOR_RGB2BGR)
​	image = cv2.resize(image, (24, 24))
​	mean=np.mean(image)
​	std=np.std(image)
​	image=(image-mean)/max(std,1/np.sqrt(image.size))
​	image = np.expand_dims(image, axis=0).astype(np.float32)
​	'''
​	2 inference
​	'''
​	preds = sess.run(y_, feed_dict={x: image})
​	pred_label=label_dict[preds[0]]
​	ret.append(pred_label)
   return ret
</code>

### 生成镜像
准备好以上文件之后，我们可以通过cifar\_infer.Dockerfile生成Docker镜像。
<code>
sudo docker build -t uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/cifar_infer_simple:latest -f cifar_infer.Dockerfile .
</code>

### 本地测试
得到镜像之后，我们可以在本地进行测试。
<code>
sudo docker run -it -p 8080:8080 

uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/cifar_infer_simple:latest
</code>
我们在./test\_images/放置了用于测试的图像，进入放置了测试图像的文件夹：
<code>
curl -X POST http://localhost:8080/service -T deer.png
</code>
命令行输出该图像中的物体类别deer，则测试成功。

### UAI-Inference平台测试
可以查看部署在线服务的[具体操作步骤](uai-inference/tutorial/tf-mnist/gpu-inference)。
当部署完毕之后，我们可以在详细页面获取CNN在线服务的URL地址。

进入放置了测试图像的文件夹，我们可以通过如下命令来测试，这里的URL即为这个在线服务的URL地址：
<code>
curl -X POST http://<URL>/service -T deer.png
</code>
命令行输出该图像中的物体类别deer，则测试成功。

