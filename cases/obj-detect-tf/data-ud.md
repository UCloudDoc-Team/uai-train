{{indexmenu_n>3}}

# 自定义数据

如使用本例中[[ai:uai-train:cases:obj-detect-tf:data|原始数据准备]]提供的图片数据集，则无需进行本节内容。以下流程将介绍如何使用自己的图片和数据构建数据集。
## 数据收集和记录
认定需要识别的物体种类后，为每个种类获取若干图片。图片应仅包含一个清晰可见的物体，不包含其他种类的物体。
物体所占区域面积至少应为50像素*50像素。图片面积较大、数量足够多，物体较清晰时，模型识别能力较强。

为每一张图片生成一个xml文件储存事实数据。xml的文件名记录了图片的类别，xml的文件内容记录图片名称、大小和物体位置。本例中所用文件格式：
<code>
<annotation>

​        <filename>Persian_01.jpg</filename>
​	<size>
​		<width>600</width>
​		<height>400</height>
​		<depth>3</depth>
​	</size>
​	<segmented>0</segmented>
	<object>
		<name>cat</name>
		<pose>Frontal</pose>
		<truncated>0</truncated>
		<occluded>0</occluded>
		<bndbox>
			<xmin>333</xmin>
			<ymin>72</ymin>
			<xmax>425</xmax>
			<ymax>158</ymax>
		</bndbox>
		<difficult>0</difficult>
	</object>
</annotation>
</code>

其中需要自定义的数据为：size记录图片的宽高像素数，bndbox记录物体位置的上边界，左边界，下边界和右边界。其他数据按如上填入即可（name可任意填入，仅用于信息记录，不用于模型训练）。

## 数据整理和打包
整理好以上图片和事实数据集后，为每对图片-数据按规则命名。文件名称的格式为：[类型]\_[数字序号].jpg/xml。

如：Persian\_01.jpg, Persian\_01.xml. 类型名为纯英文和下划线组合，序号为纯数字。图片和xml文件名前缀应完全相同。

创建文档并命名为trainval.txt，指示图片集中的哪些图片被用于训练。每行记录一张图片的前缀。例如：
<code>
Persian_01

Persian_02
basset_hound_01
</code>
在线训练将仅采用文档中记录了前缀的图片，文档中未记录的图片数据将不被采用。

创建文档并命名为label_map.pbtxt，指示所有物体类别的序号：

<code>
item {

  id: 1
  name: 'Persian'
}
item {
  id: 2
  name: 'basset_hound'
}
</code>

此文件为类别标签文件，记录了物体识别中可能出现的所有类别，并赋予序号。

将数据保存在统一根目录下（此处为/object-prep/）并记录根目录路径，以进行下一步预处理。
<code>
/object-prep/[路径]

/object-prep/label_map.pbtxt
/object-prep/annotations/[路径]
/object-prep/annotations/trainval.txt
/object-prep/annotations/xmls/[路径]
/object-prep/annotations/xmls/Persian_01.xml
/object-prep/annotations/xmls/Persian_02.xml
/object-prep/annotations/xmls/basset_hound_1.xml
/object-prep/images/[路径]
/object-prep/images/Persian_01.jpg
/object-prep/images/Persian_02.jpg
/object-prep/images/basset_hound_1.jpg
</code>

转至[[ai:uai-train:cases:obj-detect-tf:data-trans|数据格式转换]]进行文件预处理。