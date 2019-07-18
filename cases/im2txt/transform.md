{{indexmenu_n>4}}

====== 文件格式转换 ======

模型训练接收tf-record格式的图片数据作为输入，因此我们需将准备好的图片文字数据转换为tfrecord格式。此前我们已经安装了tensorflow, numpy, nltk工具，并下载或自行准备了图片文字数据。从uai-sdk工具包中获取文件转脚本[[https://github.com/ucloud/uai-sdk/blob/master/examples/tensorflow/train/im2txt/build_mscoco_data.py|github - uaisdk 文件转换工具]]，并放在数据目录下：

	
	# /data/im2txt/data
	|_ build_mscoco_data.py
	|_ train2014
	|_ val2014
	|_ annotations
	|  |_ captions_train2014.json
	|  |_ captions_val2014.json

注意此处的文件名为下载的数据的默认路径和文件名，如果使用自定义图片和文字数据作为输入则名字可能有所不同，参见[[ai:uai-train:cases:im2txt:prep-ud|准备自定义数据]]。在数据路径中，运行脚本以生成tfrecord文件：

<code>
python build_mscoco_data.py --train_image_dir /data/im2txt/data/train2014 --val_image_dir /data/im2txt/data/val2014 --train_captions_file /data/im2txt/data/captions_train2014.json --val_captions_file /data/im2txt/captions_val2014.json --output_dir /data/im2txt/data/
</code>

注意命令中的各个路径均为下载数据的默认路径，如果使用自定义数据，需将四个参数分别设置为train和val图片集和文字标注集的路径。脚本执行结束后，在数据路径中应当有如下文件：

    # cd /data/im2txt/data/
    # ls
    train-00000-of-00256 train-00001-of-00256 ... train-00255-of-00256
    test-00000-of-00008 test-00001-of-00008 ... test-00007-of-00008
    val-00000-of-00004 val-00001-of-00004 ... val-00001-of-00004
    
共计268个文件。其中，train文件为训练所用文件，test和val文件为验证和测试所用数据文件。 除图片文字数据外，还需要inception v3版物体分类模型来初始化图片文字标注模型。用wget工具下载原始模型并解压：

<code>
wget download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz
tar -xvf inception_v3_2016_08_28.tar.gz
</code>

等待下载和解压完成。可以在当前目录下找到inception_v3.ckpt文件，同样将此文件保存至/data/model/data下。此时路径下已保存的文件有：

	# cd /data/im2txt/data/
	# ls
      inception_v3.ckpt
      train-00000-of-00256 train-00001-of-00256 ... train-00255-of-00256
      test-00000-of-00008 test-00001-of-00008 ... test-00007-of-00008
      val-00000-of-00004 val-00001-of-00004 ... val-00001-of-00004
      
此时数据已准备好进行训练。