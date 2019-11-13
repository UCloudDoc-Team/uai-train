

# TF-Slim介绍
TF-Slim是TensorFlow的一个轻量级库，用于定义，训练和评估TensorFlow中的复杂模型。使用TF-Slim可以帮助我们减少样板代码的书写，从而增强代码的可读性和可维护性，同时TF-Slim的组件可以与本地tensorflow以及其他框架（如tf.contrib.learn）自由组合，具有很大的灵活性。

[TF-Slim](https://github.com/tensorflow/models/tree/master/research/slim)中提供了几种广泛使用的卷积神经网络（CNN）图像分类模型，如vgg、resnet等。
利用这些已经定义好的模型，我们可以很方便地使用自己的数据从头开始训练，或从预先训练的网络权重中对其进行微调。

## 本文提要
在本案例中，我们展示了如何使用TF-Slim中提供的图像分类模型（vgg19、resnet\_v2\_101等）来对自己的数据进行训练（从头开始或者fine-tune）。

本案例中使用的数据来自人脸表情识别数据集——fer2013，该数据集中共有七种类别，分别是angry、disgust、fear、happy、neutral、sad、surprise。

