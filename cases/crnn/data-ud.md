

# 自定义数据
该章节讲述如何构建自己的训练数据集。如果你使用[原始数据准备](ai/uai-train/cases/crnn/data)中的数据集，则可以跳过该章节。

## 数据图片格式

你的数据图片应该符合下列格式要求：
  * 每张图片上只有一行字符；
  * 由于在将图片输入网络时，图片大小会被调整为（32，100，3），如果你的图像大小能够近似按照这个比例，可以获得相对较好的训练效果；
  * 图片中不建议出现大量无关背景，文本部分应该为图片的主要内容；
下面为图片示例：

![](/ai/uai-train/images/case/crnn/example.png)

![](/ai/uai-train/images/case/crnn/tensor.png)

这两张图片对应的标签为“1 import numpy”，“tensorflow”.

## 建立sample.txt文件

你需要分别建立两个sample.txt文件，分别放置于Test和Train文件夹下，Test/sample.txt中放置了测试图像的相对路径和标签，Train/sample.txt中放置了训练图像的相对路径和标签。下面将对sample.txt中的内容进行详细介绍。

假设你将你的图像数据放置于/data/image/路径下,我们假设其中两张图像的图像名分别为test1\_image.jpg和test2\_image.jpg，其对应标签分别为label1和label2：
<code>
\_ data\

   |_ image\
     |_ test1_image.jpg
     |_ test2_image.jpg
     |_ ...
   |_ Test\
     |_ sample.txt
   |_ Train
     |_ sample.txt
</code>
则这两张图像在sample.txt中的相应存放内容为：
<code>
/image/test1_image.jpg  label1

/image/test2_image.jpg  label2
</code>
注意这里的图像路径为图像相对于Test或Train的相对路径。

## 建立字典文件
我们需要建立两个字典文件char\_dict.json和ord\_map.json，用于对网络输入和输出的字符进行编码和解码。

在[UAI-Train案例](ai/uai-train/cases/crnn/intro)提供的github地址中已经给出了两个建立好的字典文件。
**注意：** 根据提供的字典文件对应可以识别的字符列表如下：

| 可识别字符                                                |
|----------|
| 数字0，1，2，3，4，5，6，7，8，9                                |
| "%",  "'",  "*",  "+",  ",",  "-",  ".",  "/",  ":"  ，" "|
| a--z 共26个英文字符                                        |

如果你要添加自己的字符，需要修改字典文件，可以通过运行如下add.py文件添加字符：
<code>
import argparse
import json

parser = argparse.ArgumentParser()
parser.add_argument('--add_str', type=str, help='char you want to add')
parser.add_argument('--char_dict', type=str, help='path of char_dict')
parser.add_argument('--ord_map', type=str, help='path of ord_map')
args = parser.parse_args()

def add_str(s,chardict,ord_map):
    ord_s=ord(s)
    if  65 <= ord_s <= 90:
        ord_s += 32
    with open(ord_map,'r') as load_ord:
         ord_dict = json.load(load_ord)
         load_ord.close()
    ord_dict[str(len(ord_dict))]=ord_s
    new_ord_dict=json.dumps(ord_dict)
    with open(ord_map,'w') as fw:
         fw.write(new_ord_dict)
         fw.close()
    with open(chardict,'r') as load_dict:
         char_dict = json.load(load_dict)
         load_dict.close()
    char_dict[ord_s]=s
    new_char_dict=json.dumps(char_dict)
    with open(chardict,'w') as fw:
         fw.write(new_char_dict)
         fw.close()
		 
add_str(args.add_str,args.char_dict,args.ord_map)
</code>

以添加字符"&"为例，通过下列命令则将字符"&"添加到了字典中。
<code>
python  add.py --add_str='&' --char_dict='/data/char_dict.json'  --ord_map='/data/ord_map.json'
</code>

