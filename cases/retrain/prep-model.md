{{indexmenu_n>3}}
====== 模型准备 ======
重训练基于已经训练好的物体识别模型。具体理论和分析参阅[[https://www.tensorflow.org/tutorials/image_retraining|Tensorflow Retraining]]，本例中我们使用以上链接中Tensorflow提供的物体识别模型。

UAI-Train训练平台无互联网权限，因此模型需保存并上传至UFile。我们使用Tensorflow\_hub模块自带的下载组件缓存模型（[[https://www.tensorflow.org/hub/modules/image|Tensorflow Retraining: Models]]）。Tensorflow\_hub模组中的代码在加载在线获取的模型时会将模型缓存在本地默认目录下，我们可以修改此目录以将模型保存在我们想要的路径下。

  - 在本地主机安装Tensorflow\_hub：

<code>
sudo pip install tensorflow_hub
</code>

  - 转至图片保存的根目录（请参阅前一节查看保存图片的根目录，例如"/data/pets"）：

<code>
cd /data/pets
</code>

  - 将默认模型缓存目录转至当前目录（注意，此设置仅在本对话中有效。若重新连接本地主机以下载模型，则需重新设置此路径）：

<code>
export TFHUB_CACHE_DIR=/data/checkpoint_dir
</code>

  - 创建文件，编写简单脚本缓存模型（此处链接下载的为mobilenet_v1_100_224模型，你可以参考上方链接获取其他模型）：

<code>
vim cache_module.py
输入：
import tensorflow_hub as hub
m = hub.Module("https://tfhub.dev/google/imagenet/mobilenet_v1_100_224/classification/1")
</code>

等待脚本运行完成。此时模型已经保存在checkpoint\_dir路径下的一个子路径中，子路径的名字为模型数据的哈希值（此处为mobilenet\_v1\_1.0\_224模型：cc14ad57953629a2bbc0ffe52de5afb5518150b2），不同的模型此路径的名称不同。方便起见，我们将此路径下的所有文件和文件夹放入checkpoint\_dir路径中。此时准备好的图片和模型路径应如：

<code>
|_ data
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