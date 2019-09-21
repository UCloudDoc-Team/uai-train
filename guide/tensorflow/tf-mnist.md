{{indexmenu_n>6}}

# TF-MNIST开发案例

本案例所使用的模型和代码基于Tensorflow 教程MNIST案例（https://www.tensorflow.org/tutorials/mnist/beginners/），您可以在https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/mnist_summary_1.1下面下载完整的代码

## 准备工作
请根据 开发指南>Tensorflow开发指南>Tensorflow 本地安装部署开发环境[[ai:uai-train:guide:tensorflow:local]]完成所有安装步骤，即完成了基本环境的部署 
从https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/mnist_summary_1.1下载完整的案例，包括：

  * code/mnist_summary.py 训练代码
  * data/ 测试数据集

## 编写MNIST案例
本案例使用的MNIST模型的训练代码来自Tensorflow的MNIST案例的mnist_summary.py, 并做如下修改 

### 新增import uai-train的uflag.py包
添加代码引入uflag.py
<code>
from uaitrain.arch.tensorflow import uflag 
</code>

案例中修改代码如下\(L32\)：
<code>
from tensorflow.examples.tutorials.mnist import input_data

from uaitrain.arch.tensorflow import uflag

tf.app.flags.DEFINE_float('--learning_rate', 0.001, 'Initial learning rate')
tf.app.flags.DEFINE_float('--dropout', 0.9, 'Keep probability for training dropout.')
</code>

### 使用 data_dir 作为输入路径 ###
通过使用 FLAGS.data\_dir 指定输入路径
<code>
mnist = input_data.read_data_sets(FLAGS.data_dir,
                                    one_hot=True)
</code>

案例中修改代码如下\(L44\)：
<code>
def train():
  \# Import data
  mnist = input_data.read_data_sets(FLAGS.data_dir,
                                    one_hot=True)

  sess = tf.InteractiveSession()
</code>

### 使用 output_dir 作为输出路径 ###
通过使用 FLAGS.output\_dir 指定输出路径
<code>
save_path = saver.save(sess, FLAGS.output_dir + "/model.ckpt")
</code>

案例中修改代码如下\(L180\)：
<code>

  save_path = saver.save(sess, FLAGS.output_dir + "/model.ckpt")
  print("Model saved in file: %s" % save_path)
  train_writer.close()
  test_writer.close()
</code>

### 使用 log_dir 作为TensorBoard输出路径 ###
通过使用 FLAGS.log\_dir 指定TensorBoard Summary 输出路径
<code>
  train_writer = tf.summary.FileWriter(FLAGS.log_dir + '/train', sess.graph)
  test_writer = tf.summary.FileWriter(FLAGS.log_dir + '/test')
</code>

案例中修改代码如下\(L144, L145\)
<code>

# Merge all the summaries and write them out to /tmp/tensorflow/mnist/logs/mnist_with_summaries (by default)
  merged = tf.summary.merge_all()
  train_writer = tf.summary.FileWriter(FLAGS.log_dir + '/train', sess.graph)
  test_writer = tf.summary.FileWriter(FLAGS.log_dir + '/test')
  tf.global_variables_initializer().run()
</code>

### 使用 max_step 作为终止条件
通过使用 FLAGS.max\_step 最为终止条件
<code>
for i in range(FLAGS.max_step):
</code>

案例中修改代码如下\(L160\)
<code>
saver = tf.train.Saver()
  for i in range(FLAGS.max_step):
    if i % 10 == 0:  # Record summaries and test-set accuracy
      summary, acc = sess.run([merged, accuracy], feed_dict=feed_dict(False))
      test_writer.add_summary(summary, i)
      print('Accuracy at step %s: %s' % (i, acc))
</code>

## 测试MNIST案例
**确保本地已经安装TensorFlow 1.1，并且将examples/tensorflow/train/mnist\_summary\_1.1/data 下的测试数据放入 /data/data目录下，并确定/data/output 目录存在** 
执行以下代码执行训练任务
<code>
python mnist_summary.py --max_step=100 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/
</code>

## 使用Docker镜像进行测试
因为UAI Train 平台使用Docker运行训练，因此可以使用Docker对训练代码进行测试。
UAI Train平台提供开源的Docker 打包工具，打包方法见[[ai:uai-train:guide:tensorflow:packing]] 
打包完成后打包工具会自动生成GPU的镜像和CPU的镜像，命名如下：

  * **CPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>-cpu:<usr-def-tag>
  * **GPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>:<usr-def-tag>

### 使用CPU Docker 测试
打包工具会自动生成CPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for CPU local test: <docker run cmd> ）
直接执行命令即可

### 使用GPU Docker 测试
打包工具会自动生成GPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for GPU local test: <docker run cmd> ）
直接执行命令即可

