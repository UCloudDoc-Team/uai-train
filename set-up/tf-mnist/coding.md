{{indexmenu_n>7}}

===== 训练代码简介 =====
本代码基于TensorFlow 1.1 的mnist with summaries例子实现[[https://github.com/tensorflow/tensorflow/blob/v1.1.0/tensorflow/examples/tutorials/mnist/mnist_with_summaries.py]]，并做了部分改动。

=== 引入 UAI Train相关参数 ===
mnist\_summary.py#L34 import uaitrain官方flags
<code>
from uaitrain.arch.tensorflow import uflag
</code>

=== 指定输入数据地址 ===
mnist\_summary.py#L44 使用uaitrain.arch.tensorflow.uflag中的定义的FLAGS.data\_dir参数作为输入数据集
<code>
  # Import data
  mnist = input_data.read_data_sets(FLAGS.data_dir,
                                    one_hot=True)
</code>

=== 定义算法网络和summary ===
mnist\_summary.py#L51 ～ #L143

=== 指定tensorboard summary路径 ===
mnist\_summary.py#L144,145 使用uaitrain.arch.tensorflow.uflag中的定义的FLAGS.log\_dir参数作为tensorboard summary输出的路径
<code>
  merged = tf.summary.merge_all()
  train_writer = tf.summary.FileWriter(FLAGS.log_dir + '/train', sess.graph)
  test_writer = tf.summary.FileWriter(FLAGS.log_dir + '/test')
  tf.global_variables_initializer().run()
</code>

=== 执行训练 ===
mnist\_summary.py#L146-178

=== 保存训练模型 ===
mnist\_summary.py#L180 使用uaitrain.arch.tensorflow.uflag中的定义的FLAGS.output\_dir参数作为训练输出的路径
<code>
  save_path = saver.save(sess, FLAGS.output_dir + "/model.ckpt")
  print("Model saved in file: %s" % save_path)
</code>

完整代码请参见[[https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/train/mnist_summary_1.1/code/mnist_summary.py]]