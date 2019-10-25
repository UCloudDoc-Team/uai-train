

# 数据准备
本章节介绍了你该如何使用[[http://thuocl.thunlp.org/|清华大学开放中文词库的诗词数据集]]生成训练所需的数据文件。

在本章节我们需要生成四个数据文件，分别是1 训练图片数据；2 标签文件；3 字符文档；4 char_dict文件

## 准备
你需要将你下载的诗词数据集和字体文件（微软雅黑、宋体等）放置在本地/data/data/gen_data/文件夹下。

## 生成训练图片
我们需要根据诗词数据集自己生成训练图片用于模型的训练，为了给训练集增加一些随机性以增强模型的泛化能力，我们对图片进行了角度旋转、添加噪声等操作。
你可以根据下列命令生成训练图片。
<code>
sudo docker run -it -v /data/data:/data/data uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest /bin/bash -c "python /data/code/gen_data/gen_pic.py"
</code>

  * 其中--size设置了你要生成的字符大小，默认设置为30.

你将会在本地/data/data/pic文件夹下得到生成的图像文件。
## 生成标签文件
我们需要形成两个sample.txt文件，分别存放于Test和Train文件夹中。Train/sample.txt和Test/sample.txt文件中给出了需要用于训练和测试的图片的相对路径以及图片的对应标签。下面给出了sample.txt的文件内容示例。

<code>
pic/7826/11_0.jpg 只将菱角与鸡头
pic/1679/14_0.jpg 一日难再晨
</code>

你可以根据下列命令生成标签文件。
<code>
sudo docker run -it -v /data/data:/data/data uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest /bin/bash -c "python /data/code/gen_data/gen_sample.py"
</code>

生成的sample.txt中，"pic/7826/11\_0.jpg"给出了该图片相对于文件夹Train/的相对路径，"只将菱角与鸡头"给出了该图片上的字符文本。

你将会在本地的/data/data/Train和/data/data/Test下得到生成的文件。
## 生成字符文档
我们需要生成一个文档来存放诗词数据集中的所有字符。
你可以根据下列命令生成字符文档。
<code>
sudo docker run -it -v /data/data:/data/data uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest /bin/bash -c "python /data/code/gen_data/gen_chinesetxt.py"
</code>
该字符文档将用于我们后续生成char\_dict文件。

你将会在本地/data/data下得到生产的chinese.txt文件。
## 生成char_dict文件
在进行文字识别之前，我们首先需要对字符进行编码。我们将会在char\_dict文件夹下生成char\_dict.json、index\_2\_ord\_map.json和ord\_2\_index\_map.json三个文件，这三个文件描述了字符的编码和训练中index的关系。

通过下列命令我们可以在本地/data/data下生成char\_dict文件。
<code>
sudo docker run -it -v /data/data:/data/data uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest /bin/bash -c "python /data/code/tools/establish_char_dict.py"
</code>

## 使用自己的数据集
如果你要使用自己的数据集，你有以下注意事项：

1. 你需要修改/data/code/global\_configuration/config.py中的C.TRAIN.CLASSES_NUMS参数：
     C.TRAIN.CLASSES\_NUMS=chinese.txt中的字符个数+1
2. 标签长度不能超过25.