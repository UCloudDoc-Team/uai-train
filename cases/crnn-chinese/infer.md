

# 在线服务
我们可以将[UAI-Train](ai/uai-train/cases/crnn-chinese/train)得到的CRNN模型部署为在线服务来帮助我们更加方便地使用字符识别功能。我们借助Docker镜像来完成在线服务的部署。

我们提供了一个封装好的Docker镜像，可以通过如下命令拉取:
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/crnn-poem-infer:latest
</code>
下载到云主机之后，你需要重新docker tag成你自己uhub镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/crnn-poem-infer:latest，并提交至uhub。

你也可以根据下列步骤自己生成一个Docker镜像。
## 数据准备
如果你使用了我们提供的镜像，则可以跳过这一步。

我们需要建立如下的文件结构：
  * code/中放置了CRNN的模型代码文件；
  * inference/中放置了模型推理文件ocr\_inference.py；
  * data/中放置了[UAI-Train案例](ai/uai-train/cases/crnn-chinese/data)中生成的char_dict相关文件；
  * inference/checkpoint\_dir/中放置了[UAI-Train案例](ai/uai-train/cases/crnn/train)得到的模型文件；

<code>
|_ code/

|_ data/
  |_ char_dict.json  
  |_ index_2_ord_map.json  
  |_ ord_2_index_map.json
|_ inference/
   |_ ocr_inference.py 
   |_ checkpoint_dir/
|_ ocr.conf
|_ ocr.Dockerfile
</code>

### ocr.conf介绍
<code>
{                                                                                                              
	"http_server" : {                                                                                              
		"exec" : {                                                                                           
			"main_class": "ocrModel",                                                                              
			"main_file": "ocr_inference"                                                                           
		},                                                                                                  
		"tensorflow" : {                                                                                       
			"model_dir" : "./checkpoint_dir"                                                                       
		}                                                                                                 
	}                                                                                                        
} 
</code>

  * 其中exec.main\_file定义了入口python模块:ocr\_inference.py（注：这边需要将.py 删除，因为django会以模块的形式import ocr\_inference）
  * 其中exec.main\_class定义了推理服务的对象类：ocrModel 
  * 其中tensorflow.model\_dir定义了模型的相对路径 

### ocr.Dockerfile介绍

<code>
FROM uhub.service.ucloud.cn/uaishare/cpu_uaiservice_ubuntu-16.04_python-3.6.2_tensorflow-1.3.0:v1.2

EXPOSE 8080
ADD ./inference/ /ai-ucloud-client-django/
ADD ./ocr.conf  /ai-ucloud-client-django/conf.json
ADD ./code /data/code
ADD ./data/  /data/data/char_dict/
ENV UAI_SERVICE_CONFIG /ai-ucloud-client-django/conf.json
CMD cd /ai-ucloud-client-django && gunicorn -c gunicorn.conf.py httpserver.wsgi
</code>

  * 其中定义了使用的基础镜像uhub.service.ucloud.cn/uaishare/cpu\_uaiservice\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.3.0:v1.2
  * export 8080端口
  * 将当前目录下CRNN\_Tensorflow/下的文件复制到镜像中的/ai-ucloud-client-django/
  * 将ocr.conf 放入镜像中的/ai-ucloud-client-django/conf.json
  * 将./code/下的文件放入镜像中的/data/code
  * 将./data/下的文件放入镜像中的/data/data/char\_dict/
  * 指定UAI Inference server在启动时使用/ai-ucloud-client-django/conf.json 配置文件
  * 启动http server

### ocr_inference.py介绍
ocr\_inference.py首先需要实现一个在线服务的类，该类继承了TFAiUcloudModel（TensorFlow 在线服务基类）. ocr\_inference.py实现了load\_model和execute两个函数。

**1. load\_model(self)**:

该函数加载了训练好的crnn模型。
<code>
def load_model(self):
    sess = tf.Session()
    x = tf.placeholder(dtype=tf.float32, shape=[1, 32, 100, 3], name='input')
    '''
    1 define model
    '''
    net=crnn_model.ShadowNet(phase=phase_tensor, hidden_nums=256, layers_nums=2, seq_length=15, num_classes=config.cfg.TRAIN.CLASSES_NUMS, rnn_cell_type='lstm')
    with tf.variable_scope('shadow'):
       net_out, tensor_dict = net.build_shadownet(inputdata=x)
    decodes, _ = tf.nn.ctc_beam_search_decoder(inputs=net_out, sequence_length=20*np.ones(1), merge_repeated=False)

    sess_config = tf.ConfigProto()
    sess = tf.Session(config=sess_config)
    '''
    2 load model from self.model_dir
    '''
    saver = tf.train.Saver()
    params_file = tf.train.latest_checkpoint(self.model_dir)
    saver.restore(sess=sess, save_path=params_file)
    '''
    3 Register ops into self.output dict.
      So func execute() can get these ops
    '''
    self.output['sess'] = sess
    self.output['x'] = x
    self.output['y_'] = decodes
</code>

**2. execute(self, data, batch\_size)** :

该函数从data获取batching的请求数据，并转化为numpy list；通过sess.run请求推理操作；将请求结果转化成string，并合并成ret（ret也是一个list，和data list是一一对应的关系）
<code>
    def execute(self, data, batch_size):

​        sess = self.output['sess']
​        x = self.output['x']
​        y_ = self.output['y_']
​        decoder = data_utils.TextFeatureIO()
​        ret = []
​        for i in range(batch_size):
​            '''
​            1 load data 
​            '''
​            image = np.array(Image.open(data[i]))
​            image = cv2.cvtColor(np.asarray(image),cv2.COLOR_RGB2BGR)
​            image = cv2.resize(image, (100, 32))
​            image = np.expand_dims(image, axis=0).astype(np.float32)
​            '''
​            2 inference
​            '''
​            preds = sess.run(y_, feed_dict={x: image})
​            preds = decoder.writer.sparse_tensor_to_str(preds[0])[0]+'\n'
​            ret.append(preds)
​        return ret
</code>

## 生成镜像
准备好以上文件之后，我们可以通过ocr.Dockerfile生成Docker镜像。
<code>
sudo docker build -t uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/ocr-poem-infer:v1.0 -f ocr.Dockerfile .
</code>

## 本地测试
得到镜像之后，我们可以在本地进行测试。
<code>
sudo docker run -it -p 8080:8080 uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/ocr-poem-infer:latest
</code>
我们在/data/test\_images/放置了用于测试的图像，进入放置了测试图像的文件夹：
<code>
curl -X POST http://localhost:8080/service -T test_02.jpg
</code>
命令行输出该图像中的文本，则测试成功。
## UAI-Inference平台测试
可以在查看部署在线服务的[具体操作步骤](ai/uai-inference/tutorial/tf-mnist/inference)。
当部署完毕之后，我们可以在详细页面获取CRNN在线服务的URL地址。

进入放置了测试图像的文件夹，我们可以通过如下命令来测试，这里的URL即为这个在线服务的URL地址：
<code>
curl -X POST http://<URL>/service -T test_02.jpg
</code>
命令行输出该图像中的文本，则测试成功。

