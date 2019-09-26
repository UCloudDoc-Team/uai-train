{{indexmenu_n>2}}

# 数据准备

## 图片和事实集
{{ :ai:uai-train:case:oxford_pet.png?nolink&300 |}}
在监督学习中，用于训练的原始数据包括输入数据和事实数据。本例中以图片为输入数据，以物体类别和在图片上的位置为事实数据。本例所用的数据集种类为宠物猫狗，共37种，每种约有200张图片。
请下载以下数据作为案例数据集：

[[http://www.robots.ox.ac.uk/%7Evgg/data/pets/|The Oxford-IIIT Pet Dataset]]

宠物种类记录在一个文件中，用于训练和预测。
请下载本例中所用类别标签文件：

[[https://github.com/tensorflow/models/blob/master/research/object_detection/data/pet_label_map.pbtxt|pet_label_map.pbtxt]]

物体类别标签文件中记录了在物体识别中可能出现的所有物体种类，并分别赋予了序号。其格式为：

<code>
item {

  id: 1
  name: 'Abyssinian'
}
item {
  id: 2
  name: 'american_bulldog'
}
...
</code>

将物体类别标签文件重命名为label_map.pbtxt，放在images和annotations的根目录（此处为/object-prep/）下，并记录此根目录：

<code>
/object-prep/[路径]

/object-prep/label_map.pbtxt
/object-prep/annotations/[路径]
/object-prep/annotations/trainval.txt
/object-prep/annotations/xmls/[路径]
/object-prep/annotations/xmls/Abyssinian_01.xml
...
/object-prep/images/[路径]
/object-prep/images/Abyssinian_01.jpg
...
</code>

object-prep目录下包括了label\_map.pbtxt（物体类别标签文件），annotations（图片标注文件夹，其中xmls目录下保存了每一张训练图片对应的识别目标的描述文件xxx.xml），images（保存了所有训练用的图片）。

接下来转至[[ai:uai-train:cases:obj-detect-tf:data-trans|数据格式转换]]进行数据预处理。


**关于使用自选图片和物体种类作为识别对象，参见**：

[[ai:uai-train:cases:obj-detect-tf:data-ud|]]

