

# 案例介绍
![](/ai/uai-train/images/case/im2txt/man-swim-in-ocean-beside-tree.jpg)
	a man swim in ocean beside tree .


图片转文字标注项目介绍了如何在UAI-Train平台上训练一个为图片生成文字标注的模型。案例来自：
[[https://github.com/tensorflow/models/tree/master/research/im2txt]]

## 起源

图片转文字模型是是一个学习如何描述图片内容的深度学习模型。基于Inception v3物体分类模型，我们训练一层额外的特征提取模型并赋予它生成文字标注的能力。然后根据需要，我们对完整的模型再进行参数微调训练以继续提升准确度。更多项目内容和论文可参阅上方的链接。

本项目基于https://github.com/tensorflow/models/tree/master/research/im2txt，并引用了部分训练和推理代码。

本章内容主要有：

  * 环境和数据准备
  * 在线训练模型
  * 用模型启动在线推理