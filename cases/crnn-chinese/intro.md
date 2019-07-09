{{indexmenu_n>1}}

## 案例介绍

本案例使用CRNN模型进行中文字符识别，案例中使用的数据集是来自清华大学开放中文词库的诗词数据集。

CRNN模型帮助我们将图片中的字符转换为文本格式。该模型通过将CNN和LSTM模型结合起来，构建一个单行字符识别网络。我们可以在[CRNN论文](http://arxiv.org/abs/1507.05717)中查看模型结构的更多细节。我们可以在github的[ucloud/uai-sdk/example/tensorflow/train/crnn](https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/train/crnn)和[ucloud/uai-sdk/example/tensorflow/inference/crnn](https://github.com/ucloud/uai-sdk/tree/master/examples/tensorflow/inference/crnn)上查看相关案例代码和模型文件。

## 案例内容综述

本案例的主要内容包括：

1 生成镜像

2 原始数据准备

  - 生成训练数据集
  - 生成sample.txt文件
  - 生成字符编码文件

3 生成tfrecords文件

4 模型训练

  - 本地训练
  - 平台训练

5 在线服务

  - 在线服务代码准备
  - 生成在线服务镜像

我们提供了相应的docker镜像以供使用，你可以在具体的章节内容中查看镜像路径。当然你也可以按照案例步骤生成自己的镜像。
