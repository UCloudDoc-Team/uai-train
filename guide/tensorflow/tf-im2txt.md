{{indexmenu_n>7}}

# TF-Im2txt开发案例
本案例所使用的模型和代码基于Tensorflow 的im2txt案例开发。
详细案例可以访问 https://github.com/tensorflow/models/tree/master/im2txt

## 准备工作
请根据 开发指南>Tensorflow开发指南>Tensorflow 本地安装部署开发环境[[ai:uai-train:guide:tensorflow:local]]完成所有安装步骤，即完成了基本环境的部署。
数据集采用MSCOCO的数据集，具体获取方法可以参照https://github.com/tensorflow/models/tree/master/im2txt

## 编写Im2txt案例
从github下载https://github.com/tensorflow/models，在models/im2txt/im2txt/下面有im2txt案例的完整代码
<code>
$ cd ~/models/im2txt/im2txt/

$ ls 
BUILD  configuration.py  data/  evaluate.py  inference_utils/  inference_wrapper.py  ops/  run_inference.py  show_and_tell_model.py  show_and_tell_model_test.py  train.py
</code>
其中和训练相关的代码包括
<code>
train.py configuration.py show_and_tell_model.py ops/
</code>
其中train.py 为训练的入口

### 代码、数据和inceptionv3 ckpt准备
**代码准备**
我们统一将训练相关的代码放入 /data/im2txt 目录下，为了训练能通过 python train.py 执行，我们需要对目录结构进行一些调整：
<code>
/data/im2txt/train.py

​                   im2txt/__init__.py
​                             configuration.py
​                             show_and_tell_model.py
​                             ops/__init__.py
​                                   image_embedding.py
​                                   image_processing.py
​                                   inputs.py
</code>
我们根据train.py代码创建了im2txt目录，并在下面增加了\_\_init\_\_.py，并将ops目录移动至im2txt目录下，并同时添加\_\_init\_\_.py，此时train.py就可以通过
<code>
from im2txt import configuration

from im2txt import show_and_tell_model
</code>
执行了。

**数据准备**
我们可以使用models/im2txt/im2txt/data/download\_and\_preprocess\_mscoco.sh 下载并处理mscoco的数据集。
我们将数据集放入 /data/im2txt\_data/ 目录下。

**inception v3 checkpoint准备**
我们通过wget "http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz" 获取inception v3的checkpoint，并解压。
我们将checkpoint 放入 /data/im2txt/ 下
**注：我们需要把checkpoint文件和代码文件放在同一个目录下，以便一起打包到Docker下**

### Im2txt train.py 代码修改
**imort uai-train 相关SDK包(L25)**
新增代码：from uaitrain.arch.tensorflow import uflag 
<code>
from im2txt import configuration
from im2txt import show_and_tell_model
from uaitrain.arch.tensorflow import uflag 
</code>

**修改input、output和checkpoint路径**
由于uai-train对训练任务的代码路径、输入路径和输出路径有特殊规范，因此需要对代码进行微调。
微调如下：

  * 修改model\_config.input\_file\_pattern（L49），
直接从FLAGS.input\_file\_pattern 改为由FLAGS.data\_dir + FLAGS.input\_file\_pattern拼接（data\_dir为/data/data)
  * 修改model\_config.inception\_checkpoint\_file （L50）
直接从FLAGS.inception\_checkpoint\_file 改为由FLAGS..work\_dir + FLAGS.inception\_checkpoint\_file拼接（work\_dir为/data)
  * 修改train\_dir（L54）
直接从FLAGS.train\_dir改为FLAGS.output\_dir

<code>
def main(unused_argv):
    assert FLAGS.input_file_pattern, "--input_file_pattern is required"
    #assert FLAGS.train_dir, "--train_dir is required"
    assert FLAGS.output_dir, "--output_dir is required"
    
    model_config = configuration.ModelConfig()
    #model_config.input_file_pattern = FLAGS.input_file_pattern -> FLAGS.data_dir + FLAGS.input_file_pattern
    #model_config.inception_checkpoint_file = FLAGS.inception_checkpoint_file -> FLAGS.work_dir + FLAGS.inception_checkpoint_file
    model_config.input_file_pattern = FLAGS.data_dir + FLAGS.input_file_pattern
    model_config.inception_checkpoint_file = FLAGS.work_dir + FLAGS.inception_checkpoint_file
    training_config = configuration.TrainingConfig()
    
    # Create training directory.
    #train_dir = FLAGS.train_dir -> FLAGS.output_dir
    train_dir = FLAGS.output_dir
    if not tf.gfile.IsDirectory(train_dir):
      tf.logging.info("Creating training directory: %s", train_dir)
      tf.gfile.MakeDirs(train_dir)
</code>

### Im2txt 镜像打包
**Cmdline参数**
由于我们对input\_file\_pattern、inception\_checkpoint\_file的语义进行了修改，因此我们对程序的cmdline参数也需要进行修改。
<code>
train.py --input_file_pattern=/train-?????-of-00256 --inception_checkpoint_file=/inception_v3.ckpt --train_inception=false --number_of_steps=1000000 --log_every_n_steps=10
</code>

**打包Docker镜像**
详细的训练镜像打包说明请参见：[[ai:uai-train:guide:tensorflow:packing]]
<code>
sudo python tf_deploy.py pack --public_key=<YOUR_PUB_KEY> --private_key=<YOUR_PRIVATE_KEY> --code_path=im2txt/ --mainfile_path=train.py --uhub_username=<YOUR_UCLOUD_ACCOUNT> --uhub_password=<YOUR_UCLOUD_ACCOUNT_PASSWORD> --uhub_registry=<YOUR_UHUB_REGISTRY> --uhub_imagename=im2txt --ai_arch_v=tensorflow-1.1.0 --test_data_path=/data/im2txt_data/ --test_output_path=/data/im2txt_out/ --train_params="--input_file_pattern=/train-?????-of-00256 --inception_checkpoint_file=/inception_v3.ckpt --train_inception=false --number_of_steps=1000000"
</code>

打包完成后会生成：
  * uhub.service.ucloud.cn/uai\_dockers/im2txt
  * im2txt-cpu
两个镜像，可以通过如下方法测试：
  * GPU Docker 测试

<code>
sudo nvidia-docker run -it -v /mnt/im2txt_data/:/data/data -v /mnt/im2txt/output:/data/output uhub.service.ucloud.cn/demo/im2txt:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/train.py --input_file_pattern=/train-?????-of-00256 --inception_checkpoint_file=/inception_v3.ckpt --train_inception=false --number_of_steps=1000000 --log_every_n_steps=10 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>

注：uhub.service.ucloud.cn/demo/im2txt:uaitrain 为demo使用

  * CPU Docker 测试

<code>
sudo docker run -it -v /mnt/im2txt_data/:/data/data -v /mnt/im2txt/output:/data/output im2txt-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/train.py --input_file_pattern=/train-?????-of-00256 --inception_checkpoint_file=/inception_v3.ckpt --train_inception=false --number_of_steps=1000000 --log_every_n_steps=10 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>

### 提交UAI-Train
在UAI-Train平台部署时使用的命令为：(uaitrain_cmd.txt 下"CMD Used for deploying:" 后的内容）

<code>
/data/train.py --input_file_pattern=/train-?????-of-00256 --inception_checkpoint_file=/inception_v3.ckpt --train_inception=false --number_of_steps=1000000 --log_every_n_steps=10
</code>

