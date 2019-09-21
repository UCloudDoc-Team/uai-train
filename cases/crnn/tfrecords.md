{{indexmenu_n>4}}

# 数据格式转换
我们需要将数据转化为Tensorflow需要的tfrecords形式，在这里我们借助Docker镜像来生成tfrecords文件。

我们已经提供了一个docker镜像用于完成数据的转换，你可以在云主机上通过docker pull命令下载该镜像，你也可以通过Dockerfile文件自己生成镜像。
<code>
sudo docker pull uhub.service.ucloud.cn/uai_demo/crnn-generate-tfrecords:v1.0
</code>
如果你不打算自己生成镜像，则可以跳过下面的“Dockerfile文件”和“生成镜像”这两个步骤。

## 数据准备
我们可以将数据按照如下文件结构进行放置。

  * ./1/2/373\_coley\_14845.jpg是我们从Synthetic Word Dataset中解压出来的图像文件,我们将其放置在这里作为示例；
  * ./code中放置了相应的代码文件；
  * Test和Train中放置了我们准备好的用于训练和测试的sample.txt文件；
  * 注意：char\_dict.json和ord\_map.json需要放置在data/data/文件夹下(这两个文件描述了字符的编码和训练中index的关系)；
  * 注意：如果你要训练自己的图像文件，需要将图像放置于/data/data/目录下，并在sample.txt中填写图像的相对路径；
  * 注意：如果你要使用本章提供的镜像，则可以忽略/data/code和/data/crnn-generate-tfrecords.Dockerfile文件的准备；
<code>
/_ data/
  *   |_ data/
|_ char_dict.json
|_ ord_map.json
  |_ Test/
      |_ sample.txt
    |_ Train/
      |_ sample.txt
    |_ 1/
      |_ 2/
       |_ 373_coley_14845.jpg
    |_ ...
      |_ code/
	|_ crnnmodel/
    |_ data_provoder/
  |_ global_configuration/
    |_ local_utils/
    |_ tools/
      |_ output/
      |_ crnn-generate-tfrecords.Dockerfile
    </code>

### Dockerfile文件
Docker镜像build的时候会基于uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.3.0:v1.0的基础镜像，然后将code/下面的代码拷贝到docker镜像的/data/code/目录下，具体的crnn-generate-tfrecords.Dockerfile文件的代码如下：
<code>
From uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-16.04_python-3.6.2_tensorflow-1.3.0:v1.0
ADD ./code/ /data/code/
</code>

## 生成镜像
我们通过build命令来生成Docker镜像：uhub.service.ucloud.cn/uai\_demo/crnn-generate-tfrecords:v1.0，你需要将“uai\_demo”修改为 <YOUR\_UHUB\_REGISTRY>.
<code>
sudo docker build -t uhub.service.ucloud.cn/uai_demo/crnn-generate-tfrecords:v1.0 -f crnn-generate-tfrecords.Dockerfile .
</code>

## 生成tfrecords文件
得到镜像之后，我们可以借助该镜像来生成tfrecords文件。可以选择在本地或者是UAI-Train平台上生成tfrecords文件。
### 本地生成tfrecords文件
我们需要将本地的/data/data和/data/output映射到docker镜像中去
<code>
sudo docker run -it -v /data/data:/data/data -v /data/output:/data/output uhub.service.ucloud.cn/uai_demo/crnn-generate-tfrecords:v1.0  /bin/bash -c " /usr/bin/python /data/code/tools/write_text_features.py --dataset_dir /data/data/ --save_dir /data/output"
</code>
然后我们可以在本地/data/output中找到生成的tfrecords文件。
### UAI-Train平台生成tfrecords文件
1 首先我们需要把生成tfrecords文件所需的/data/data下的数据上传至UFile或是UFS平台上，这里我们以UFile平台为例。

我们使用事先下载的UFile工具，进入filemgr-linux64.elf文件夹，通过UFile平台的数据上传命令将/data/data下的数据上传至UFile中，并以/crnn/train\_data/作为前缀，你可以自由地修改上传数据的前缀；
<code>
./filemgr-linux64 --action mput --bucket uai-demo --dir /data/data/ --prefix /crnn/train_data/ 
</code>
2 如果你使用的是本文开头提供的镜像，则需要重新docker tag成你自己uhub 镜像库中的镜像，例如uhub.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/crnn-generate-tfrecords:v1.0， 并通过下列命令提交至uhub。 
如果你使用的是自己生成的镜像，可以通过下列命令直接上传到镜像容器库中。
<code>
sudo docker push uhub.service.ucloud.cn/<YOUR_UHUB_REGISTRY>/crnn-generate-tfrecords:v1.0
</code>
3 生成tfrecords文件

进入UAI-Train训练平台，选择训练相关的参数：

  * 训练镜像 uhub.service.ucloud.cn/<YOUR\_UHUB\_REGISTRY>/crnn-generate-tfrecords:v1.0（界面会自动关联你的uhub镜像库）；
  * 输入数据源（UFile）
  * 输出数据源（UFile）

训练的执行命令如下：
<code>
/data/code/tools/write_text_features.py --dataset_dir /data/data/ --save_dir /data/output/
</code>
我们可以在事先设置好的UFile输出数据地址中查看生成的tfrecords文件。




