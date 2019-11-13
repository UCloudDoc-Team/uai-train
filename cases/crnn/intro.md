

# 案例介绍
CRNN模型帮助我们将图片中的字符转换为文本格式。该模型通过将CNN和RNN模型结合起来，构建一个单行字符识别网络。我们可以在[论文](http://arxiv.org/abs/1507.05717)查看模型结构的更多细节。我们可以在github上查看相关[案例代码](github.com/ucloud/uai-sdk/example/tensorflow/inference/crnn)和[模型文件](github.com/ucloud/uai-sdk/example/tensorflow/train/crnn)。

## 案例内容综述
本案例的主要内容包括：

1. 数据准备

  * 图片数据集介绍

  * 图片标签介绍

2. 生成tfrecords文件

  * Build Docker镜像

3. 模型训练

  * Build Docker镜像

  * 本地训练

  * UAI-Train训练

4. 在线服务

  * 在线服务代码准备

  * 生成在线服务镜像

我们提供了相应的docker镜像以供使用，你可以在具体的章节内容中查看镜像路径。当然你也可以按照案例步骤生成自己的镜像。

