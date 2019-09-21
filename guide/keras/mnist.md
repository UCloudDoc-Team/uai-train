{{indexmenu_n>6}}

# MNIST开发案例

本案例所使用的模型和代码基于Keras 的MNIST案例，您可以在https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py找到Keras原始的案例。您可以在您可以在https://github.com/ucloud/uai-sdk/tree/master/examples/keras/train/mnist/下面下载完整的代码和测试数据集。

## 准备工作
请根据 开发指南>Keras开发指南>Keras 本地安装部署开发环境[[ai:uai-train:guide:keras:local]]完成所有安装步骤，即完成了基本环境的部署 
从https://github.com/ucloud/uai-sdk/tree/master/examples/keras/train/mnist下载完整的案例，包括：

  * code/ 训练代码，包括mnist_cnn.py mnist_datasets.py
  * data/ 测试数据集

## 编写MNIST案例
本案例使用的MNIST模型的训练代码来自Keras的MNIST案例的mnist_cnn.py和mnist_datasets.py, 并做如下修改 

### mnist_cnn.py
#### 新增import uai-train的uflag.py包
UAI Train的Keras的backend为TensorFlow，因此我们直接引用TensorFlow的uflag.py包 
**添加代码引入uflag.py**：
<code>
import tensorflow as tf
from uaitrain.arch.tensorflow import uflag

FLAGS = tf.app.flags.FLAGS
flags = tf.app.flags
</code>

案例中修改代码如下\(L15\)：
<code>
from keras import backend as K

import tensorflow as tf
from uaitrain.arch.tensorflow import uflag

FLAGS = tf.app.flags.FLAGS
flags = tf.app.flags
</code>

#### 使用 data_dir 作为输入路径
**通过使用 FLAGS.data\_dir 指定输入路径**
<code>
(x_train, y_train), (x_test, y_test) = load_data(FLAGS.data_dir)
</code>

案例中修改代码如下\(L31\)：
<code>
\# input image dimensions
img_rows, img_cols = 28, 28

\# the data, shuffled and split between train and test sets
(x_train, y_train), (x_test, y_test) = load_data(FLAGS.data_dir)
</code>

#### 使用 output_dir 作为输出路径
**通过使用 FLAGS.output\_dir 指定输出路径**
<code>
model_path = FLAGS.output_dir + '/mnist_model.h5'
model.save(model_path)
</code>

案例中修改代码如下\(L80\)：
<code>
score = model.evaluate(x_test, y_test, verbose=0)
print('Test loss:', score[0])
print('Test accuracy:', score[1])

model_path = FLAGS.output_dir + '/mnist_model.h5'
model.save(model_path)
</code>

#### 使用 log_dir 作为TensorBoard输出路径
**通过使用 FLAGS.log\_dir 指定TensorBoard Summary 输出路径**
<code>
tbCallBack = keras.callbacks.TensorBoard(log_dir=FLAGS.log_dir, histogram_freq=0, write_graph=True, write_images=True)
</code>

案例中修改代码如下\(L69\)
<code>
model.compile(loss=keras.losses.categorical_crossentropy,
              optimizer=keras.optimizers.Adadelta(),
              metrics=['accuracy'])
tbCallBack = keras.callbacks.TensorBoard(log_dir=FLAGS.log_dir, histogram_freq=0, write_graph=True, write_images=True)

model.fit(x_train, y_train,
          batch_size=batch_size,
          epochs=epochs,
          verbose=1,
          validation_data=(x_test, y_test),
          callbacks=[tbCallBack])
</code>

## 使用Docker镜像进行测试
因为UAI Train 平台使用Docker运行训练，因此可以使用Docker对训练代码进行测试。
UAI Train平台提供开源的Docker 打包工具，打包方法见[[ai:uai-train:guide:keras:packing]] 
打包完成后打包工具会自动生成GPU的镜像和CPU的镜像，命名如下：

  * **CPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>-cpu:<usr-def-tag>
  * **GPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>:<usr-def-tag>

#### 使用CPU Docker 测试
打包工具会自动生成CPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for CPU local test: <docker run cmd> ）
直接执行命令即可

#### 使用GPU Docker 测试
打包工具会自动生成GPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for GPU local test: <docker run cmd> ）
直接执行命令即可