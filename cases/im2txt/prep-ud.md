{{indexmenu_n>3}}

# 自定义数据

 本节介绍如何自行准备图片原始数据。如果自行准备数据，则无需使用前一节[[ai:uai-train:cases:im2txt:prepare|]]中下载的数据。需要注意的是训练出相对理想的模型需要极大量的、质量较高的原始数据，因此如果你没有足够多的（约数十万至数百万张）图片以及对应文字标注，本节的数据仅能供测试和体验流程所用。

获取一些图片。图片应当满足几个要求：
  - 大小适中，约在300\*300至500\*500，尽量靠近正方形；
  - 内容明确，除文字描述的内容之外不应有太多其他内容；
  - 内容清晰，被描述的内容应当较完整、面积较大。

不满足要求的图片仍可用于训练，但模型准确度会较差。收集好图片后，将约70%的图片放在数据根目录的/train/子目录下，30%的图片放在数据根目录的/val/子目录下。train目录中的图片将全部用于训练，而val目录中的图片将在每轮训练后被随机抽取验证训练效果。这次数据划分不会影响训练效果，随意大致按比例划分即可。

划分好后，需要为两组图片创建文字标注文件。首先，为每张图片准备至少一个标注。每个标注包含两个信息：
	图片的完整文件名
	图片的文字描述

文字描述应当满足几个要求：
  - 单词全部为常见英文词典；
  - 每两个单词之间都以空格分隔；
  - 语法统一，相对规范，描述图片的主要内容；
  - 语义清晰，无需参照语境即可理解内容。

例如：

{{:ai:uai-train:case:im2txt:boy-play-football.jpg?nolink&500|}}

a boy play football on green grass .

其中，要求1和2必须满足，否则在训练时此图片-文字对将被忽略。满足3和4的文字描述可以提高模型的准确度。准备好文字标注后，即可生成标注文件。标注文件的格式为：

<code>
{"images" : [{"id" : 1, "file_name" : "boy-play-skateboard.jpg"},

​		{"id" : 2, "file_name" : "man-ski-on-ocean.jpg"}, 
​		...
​		], 
"annotations" : [{"image_id" : 1, "caption" : "a boy play skateboard on playground."}, 
​		{"image_id" : 2, "caption" : "a man ski on ocean beside boat."}, 
​		...
​		]
}
</code>

其中，images为每个图片名分配一个id，annotations中每个条目为此id的图片添加一个标注。图片的名称必须在文件夹（train和val）中分别唯一，因此图片名对应的id也唯一；但可以在annotations中为同一个id添加多个文字标注，例如：

{{:ai:uai-train:case:im2txt:boy-play-football.jpg?nolink&500|}}

boy-play-football.jpg

<code>
{"images" : [	...,

​		...,
​		{"id" : 101, "file_name" : "boy-play-football.jpg"}, 
​		...
​		], 
"annotations" : [...,
​		{"image_id" : 101, "caption" : "a boy play football on green grass."}, 
​		{"image_id" : 101, "caption" : "a boy in black shirt and a boy in blue shirt play football on grass."}, 
​		...
​		]
}
</code>

单一图片的标注数量不限，只要满足以上描述的文字标注条件即可。

分别为train和val图片目录建立好以上的标注文件，分别命名为captions_train.json和captions_val.json，并放在im2txt工作目录的annotations子目录下（例子中为/data/im2txt/data/）。此时原始数据文件目录应含以下文件：

<code>
#/data/im2txt/data/

|_ train
|_ val
|_ annotations
|  |_ captions_train.json
|  |_ captions_val.json
</code>
此时原始数据已准备好进行数据格式转换。

