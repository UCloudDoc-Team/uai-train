{{indexmenu_n>2}}

# 环境和数据准备

理想的图片标注模型应当生成合乎语法的、接近图片真实内容的文字标注。要训练出理想的模型，需要大量的图片文字对作为数据集（数百万个数据对）。本节介绍如何获取一组MSCOCO图片文字数据集。

## 环境准备
由于数据集在线保存为.zip的压缩格式，我们需在本地安装unzip工具准备解压：

	sudo apt-get install unzip

下载工具可直接使用wget工具。数据格式转换所用的脚本需要用到tensorflow, numpy和nltk工具包：

	sudo pip install tensorflow
	sudo pip install -U nltk
	python -m nltk.downloader punkt



## 下载数据
转至im2txt工作目录，例如以/data/im2txt/data为数据储存路径：

	cd /data/im2txt/data

使用wget工具分别下载图片和文字标注集。图片-标注集分为两组，分别在训练中用于训练和测试。图片的下载命令为：

	wget http://msvocds.blob.core.windows.net/coco2014/train2014.zip
	wget http://msvocds.blob.core.windows.net/coco2014/val2014.zip

标注的下载命令为：

	wget http://msvocds.blob.core.windows.net/annotations-1-0-3/captions_train-val2014.zip

等待下载完成。此时，目录中应当有三个压缩文件：

	# /data/im2txt/data
	|_ train2014.zip
	|_ val2014.zip
	|_ captions_train-val2014.zip

分别用unzip解压：

	unzip train2014.zip
	unzip val2014.zip
	unzip captions_train-val2014.zip

等待解压完毕。此时，目录中应当有三个子目录：

	# /data/im2txt/data
	|_ train2014
	|_ val2014
	|_ annotations
	|  |_ captions_train2014.json
	|  |_ captions_val2014.json

此时原始文件已经准备好。进行图片标注训练时，脚本接收tf-record格式的图片-文字数据作为输入，因此我们需要运行脚本将这些数据转为tf-record格式。

