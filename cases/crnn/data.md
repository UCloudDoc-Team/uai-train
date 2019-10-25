

# 数据准备
本章节介绍了你该如何使用Synthetic Word Dataset数据集进行模型训练。如果你要使用自己的数据进行训练，可以跳过此章节查看[[ai:uai-train:cases:crnn:data-ud|使用自己的数据集]]。
## 数据集介绍
我们使用在[[http://www.robots.ox.ac.uk/~vgg/data/text/]]下载的Synthetic Word Dataset数据集进行模型的训练。将数据集解压后，我们就得到了训练数据图像。下图是一张数据图像示例。注意，文件夹中的图像为英文字符图像，且每张图像中只有一行字符串。

{{:ai:uai-train:case:crnn:test_01.jpg?nolink&300|}}

## 数据标签介绍
我们需要形成两个sample.txt文件，分别存放于Test和Train文件夹中。Train/sample.txt和Test/sample.txt文件中给出了需要用于训练和测试的图片的相对路径以及图片的对应标签。下面给出了sample.txt的文件内容示例。

<code>
path/373_coley_14845.jpg coley
path/176_Nevadans_51437.jpg nevadans
</code>

以"path/373\_coley\_14845.jpg coley"为例，其中"path/373\_coley\_14845.jpg"给出了该图片相对于文件夹Train/的相对路径，"coley"给出了该图片上的英文字符文本。

具体的路径填写方法我们在下面通过一个例子来介绍。

我们将Train/sample.txt、Test/sample.txt和训练数据图像放置于文件夹/data下。在这个例子中我们将解压得到的Synthetic Word Dataset数据文件也放置于/data路径下,其中./1/2/373\_coley\_14845.jpg是我们解压后得到的数据文件之一，在此我们不将所有图片一一列举，只列举一张图片作为例子。
<code>
/data/
   |_ Train/
     |sample.txt
   |_ Test
     |sample.txt
   |_ 1/
      |_ 2/
	   |_ 373_coley_14845.jpg

</code>
在上面的文件结构下，373\_coley\_14845.jpg这张图片在sample.txt中的对应存放内容应该为"/1/2/373\_coley\_14845.jpg  coley"
