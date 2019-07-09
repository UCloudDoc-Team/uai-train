{{indexmenu_n>3}}

## 模型训练

我们在UAI-Train平台上进行模型的训练，并基于UFS平台进行数据的存储。

### docker镜像上传

我们需要将[](/ai/uai-train/case/slim/tfrecord)中生成的docker镜像上传到UHub中：

    sudo docker push uhub.ucloud.cn/<YOUR_UHUB_REGISTRY>/slim

### 基于UFS的数据储存

你可以通过[UFS产品文档](https://cms.docs.ucloudadmin.com/storage_cdn/ufs/index)了解UFS的使用。这里我们将[](/ai/uai-train/case/slim/tfrecord)生成的tfrecord文件保存在/mnt/slim/fer/tfrecord/下，在UAI-Train平台上，数据输入路径相应填写/slim/fer/tfrecord/
数据输出路径我们设置为/slim/fer/checkpoint(该路径对应本地的/ufs/slim/fer/checkpoint)

### 基于UAI-Train平台的模型训练

**单节点的模型训练**

训练命令如下，我们可以通过修改参数model\\\_name来修改图片分类模型，你可以通过文末的表格参考模型选择。

    /data/train_image_classifier.py --batch_size=64 --model_name=mobilenet_v2_035 --num_gpus=1

\*\* 分布式训练\*\*

在UAI-Train平台中选择Tensorflow-分布式训练，运行如下训练命令即可进行分布式训练。

    /data/train_image_classifier.py --batch_size=64 --model_name=vgg_19

根据在UAI-Train平台上设置的UFS输出路径，我们可以查看得到的模型。

**基于已有的checkpoint进行Fine-tuning**

我们也可以基于预训练的模型进行Fine-tuning，在[Pre-trained
Models](https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models)中可以进行模型下载。下面以resnet\\\_v2\\\_101模型为例进行模型训练.

  - 模型下载



    cd /ufs/slim/fer/checkpoint
    sudo wget http://download.tensorflow.org/models/resnet_v2_101_2017_04_14.tar.gz
    tar -xvf resnet_v2_101_2017_04_14.tar.gz
    rm resnet_v2_101_2017_04_14.tar.gz

  - 训练



    /data/train_image_classifier.py --data_dir=/data/data/ --output_dir=/data/output/ --num_gpus=1 --model_name=resnet_v2_101 -checkpoint_exclude_scopes=resnet_v2_101/logits --trainable_scopes=resnet_v2_101/logits

## 模型评估

对得到的模型，我们可以在测试数据集上计算模型准确率、召回率等。
通过以下命令可以计算模型的准确率和召回率，其中model\\\_name应该与训练时使用的model\\\_name相同，这里的/mnt/slim/fer/checkpoint保存了训练得到的模型。

    sudo nvidia-docker run -it -v /data/fer/tfrecord:/data/tfrecord -v /mnt/slim/fer/checkpoint:/data/output /bin/bash -c "python /data/eval_image_classifier.py --checkpoint_path=/data/output  --dataset_name=fer --dataset_dir=/data/tfrecord --model_name=vgg_19"

TF-Slim中的图片分类模型列表如下：

<table>
<thead>
<tr class="header">
<th>TF-Slim中的卷积神经网络模型</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>'alexnet_v2'<br />
'cifarnet'<br />
'overfeat'<br />
'vgg\_a'<br />
'vgg\_16'<br />
'vgg\_19'<br />
'inception\_v1'<br />
'inception\_v2'<br />
'inception\_v3'<br />
'inception\_v4'<br />
'inception\_resnet\_v2'<br />
'lenet'<br />
'resnet\_v1\_50'<br />
'resnet\_v1\_101'<br />
'resnet\_v1\_152'<br />
'resnet\_v1\_200'<br />
'resnet\_v2\_50'<br />
'resnet\_v2\_101'<br />
'resnet\_v2\_152'<br />
'resnet\_v2\_200'<br />
'mobilenet\_v1'<br />
'mobilenet\_v1\_075'<br />
'mobilenet\_v1\_050'<br />
'mobilenet\_v1\_025'<br />
'mobilenet\_v2'<br />
'mobilenet\_v2\_140'<br />
'mobilenet\_v2\_035'<br />
'nasnet\_cifar'<br />
'nasnet\_mobile'<br />
'nasnet\_large'<br />
'pnasnet\_large'<br />
'pnasnet\_mobile'</td>
</tr>
</tbody>
</table>
