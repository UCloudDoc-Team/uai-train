{{indexmenu_n>4}}

# 打包镜像
UAI-Train为用户提供了镜像打包工具，用户只需将所需代码文件放在某一路径下，执行打包命令即可以生成UAI-Train所需的镜像。

该打包工具将在本地docker中生成**两个镜像**以及**运行镜像的指令说明文件uaitrain_cmd.txt**。生成的镜像包括cpu和gpu两个版本，其中gpu版本的镜像会自动上传至用户的Uhub镜像仓库。两个版本的镜像均可以用于本地测试，测试命令可在uaitrain\_cmd.txt中查询。

## Step0: 安装最新版本的UAI SDK和docker支持
安装UAI SDK的方法如下：
<code>
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
</code>

安装Docker的方法请参见：[[ai:uai-train:basic:docker]]

## Step1: 找到UAI-Train TensorFlow操作工具所在目录
<code>
$ls ~/uai-sdk/uaitrain_tool/tf
tf_tool.py
</code>

## Step2: 将训练任务所需代码放在统一路径下，打包时将其相对路径作为参数code_path上传
例如，我们要将~/uai\-sdk/examples/tensorflow/train/mnist\_summary\_1.1下面的训练代码进行打包，该文件路径结构如下：
<code>
$ cd ~/uai-sdk/examples/tensorflow/train/mnist_summary_1.1
$ ls
data  code
</code>
我们需要做如下准备工作：
  - 准备好训练的代码，案例中训练代码在mnist\_summary\_1.1/code下, 名为 **mnist\_summary.py**
  - 将tf\_tool.py 工具放入和训练代码目录同级的目录下，即mnist\_summary\_1.1/ 目录下
  - Ready To Pack
<code>
$ cd ~/uai-sdk/examples/tensorflow/train/mnist_summary_1.1
$ ls
data/ code/

$ cp ~/uai-sdk/uaitrain_tool/tf/tf_tool.py ./
$ ls
data/ code/ tf_tool.py 
</code>

## Step3: 执行pack命令，完成镜像的打包
tf\_tool.py pack命令执行方法如下：
<code>
sudo python tf_tool.py pack [-h] --public_key PUBLIC_KEY 
                        --private_key PRIVATE_KEY 
                        [--project_id PROJECT_ID] 
                        --code_path CODE_PATH 
                        --mainfile_path MAINFILE_PATH
                        --uhub_username UHUB_USERNAME
                        --uhub_password UHUB_PASSWORD 
                        --uhub_registry UHUB_REGISTRY
                        --uhub_imagename UHUB_IMAGENAME
                        [--uhub_imagetag UHUB_IMAGETAG]
                        [--internal_uhub false/true]
                        --ai_arch_v AI_ARCH_V
                        --test_data_path TEST_DATA_PATH
                        --test_output_path TEST_OUTPUT_PATH
                        --train_params TRAIN_PARAMS
                        [--python_version PYTHON_VERSION]
                        [--os OS_VERSION]
</code>

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| public\_key         | 用户的公钥                                                                                                                                                                  | 是                     |
| private\_key        | 用户的私钥                                                                                                                                                                  | 是                     |
| project\_id         | 项目id                                                                                                                                                                   | 否                     |
| code\_path          | 训练任务所需代码路，以/结尾(请填写相对tf_tool.py的相对路径，本例中为./code/)                                                                                                                       | 是                     |
| mainfile\_path      | 训练主文件（有main函数），不须包含路径，但须包含后缀名（本例中为：mnist\_summary.py）                                                                                                                  | 是                     |
| uhub\_username      | uhub账号(即UCloud账号)                                                                                                                                                      | 是                     |
| uhub\_password      | uhub密码(即UCloud账号密码)                                                                                                                                                    | 是                     |
| uhub\_registry      | uhub镜像仓库名称，不支持大写字母                                                                                                                                                     | 是                     |
| uhub\_imagename     | 生成镜像名称，不支持大写字母                                                                                                                                                         | 是                     |
| uhub\_imagetag      | 生成镜像tag，不支持大写字母                                                                                                                                                        | 否，默认为uaitrain         |
| internal\_uhub      | 告诉打包程序是否使用uhub的UCloud内网地址，如果true则使用uhub.service.ucloud.cn，false则使用uhub.ucloud.cn，使用内网地址可以提高镜像拉取和上传的速度                                                                  | 否，默认为false            |
| ai\_arch\_v         | 深度学习框架以及版本，目前支持tensorflow-1.9.0, tensorflow-1.8.0, tensorflow-1.7.0, tensorflow-1.6.0等，参见UAI Train基础镜像一览：http://cms.docs.ucloudadmin.com/ai/uai-train/general/dockers  | 是                     |
| test\_data\_path    | 本地测试环境数据输入路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                                                                                     | 是                     |
| test\_output\_path  | 本地测试环境训练输出路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                                                                                     | 是                     |
| train\_params       | 训练所需参数                                                                                                                                                                 | 是                     |
| python\_version     | python的基础版本，名称以及版本用"-"分隔，形如python-2.7.6                                                                                                                                | 否，默认为python-2.7.6     |
| os                  | OS的基础版本，名称以及版本用"-"分隔，形如ubuntu-14.04.05                                                                                                                                 | 否，默认为ubuntu-14.04.05  |

**命令样例（以tensorflow1.1.0为例）**
使用mnist\_summary\_1.1中的训练程序为案例。
test\_data\_path和test\_data\_path不要求一定在训练代码路径下，如我们可以在/data/test目录下创建了两个子目录：
  * /data/test/data 用于存放训练数据，此时test\_data\_path值为/data/test/data
  * /data/test/output 用于存放训练输出数据，此时test\_output\_path为/data/test/output 
train\_params为训练代码中使用到的任意训练参数，本例中为"\-\-max_step=2000"
使用命令时，需要使用sudo，保证docker镜像打包命令有足够权限。

注：我们可以将~/uai\-sdk/examples/tensorflow/train/mnist\_summary\_1.1/data/ 下的测试数据放入/data/test/data/目录下

<code>
$ cd ~/uai-sdk/examples/tensorflow/train/mnist_summary_1.1
$ ls
data/ code/ tf_tool.py 
$ sudo python tf_tool.py pack \
                        --public_key=<YOUR_PUBLIC_KEY> \
			--private_key=<YOUR_PRIVATE_KEY> \
			--code_path=./code/ \
			--mainfile_path=mnist_summary.py \
			--uhub_username=<YOUR_UHUB_USER_NAME> \
			--uhub_password=<YOUR_UHUB_PASSWORD> \
			--uhub_registry=<YOUR_UHUB_REFDISTRY> \
			--uhub_imagename=<YOUR_UHUB_IMAGENAME> \
                        --internal_uhub=true \
			--ai_arch_v=tensorflow-1.1.0 \
			--test_data_path=/data/test/data \
			--test_output_path=/data/test/output \
			--train_params="--max_step=2000" \
			--os=OS_VERSION \
			--python_version=PYTHON_VERSION
</code>
执行完指令后，训练镜像就已经准备好了

### 可以选镜像及相关命令组合 ###

| 镜像版本 | os   | python_version | 命令组合 |
| -------- | ---- | -------------- | -------- |
| tensorflow2.0.0a+py3(cuda10)  | ubuntu-16.04     | python-3.5      | \-\-ai\_arch\_v=tensorflow-2.0.0a \-\-python\_version=python-3.5 \-\-os=ubuntu-16.04      |
| tensorflow2.0.0a+py2(cuda10)  | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-2.0.0a \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04    |
| tensorflow1.9.0+py3(cuda9)    | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.9.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.9.0+py2(cuda9)    | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.9.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04     |
| tensorflow1.8.0+py3(cuda9)    | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.8.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.8.0+py2(cuda9)    | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.8.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04     |
| tensorflow1.7.0+py3(cuda9)    | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.7.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.7.0+py2(cuda9)    | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.7.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04     |
| tensorflow1.6.0+py3(cuda9)    | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.6.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.6.0+py2(cuda9)    | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.7.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04     |
| tensorflow1.5.0+py2(cuda9)    | ubuntu-16.04     | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.5.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-16.04     |
| tensorflow1.4.0+py3           | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.4.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.4.0+py2           | ubuntu-14.04.05  | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.4.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-14.04.05  |
| tensorflow1.3.0+py3           | ubuntu-16.04     | python-3.6.2    | \-\-ai\_arch\_v=tensorflow-1.3.0 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04     |
| tensorflow1.2.0+py2           | ubuntu-14.04.05  | python-2.7.6    | \-\-ai\_arch\_v=tensorflow-1.2.0 \-\-python\_version=python-2.7.6 \-\-os=ubuntu-14.04.05  |






## Step4: 输出说明
### 标准输出
成功执行后，界面显示样例如下，会给出部署时所需的CMD命令以及本地测试的cmd命令:
<code>
CMD Used for deploying:
/data/mnist_summary.py --max_step=2000
CMD for CPU local test:
sudo docker run -it -v /data/test/data:/data/data -v /data/test/output:/data/output tf-mnist-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
CMD for GPU local test:
sudo nvidia-docker run -it -v /data/test/data:/data/data -v /data/test/output:/data/output uhub.ucloud.cn/tensorflow-demo/tf-mnist:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>

  * **CMD Used for deploying**: 该输出的内容为创建训练任务时，**训练启动命令**框中需要填写的内容(参见[[ai:uai-train:set-up:how-to-use:create]])。可以直接复制黏贴到命令框中。
  * **CMD for CPU local test**: 该输出的内容为本地通过CPU来测试训练能否正常执行。在本地没有GPU的情况下可以使用该命令测试训练代码能否正常执行。
  * **CMD for GPU local test**：该输出的内容为本地通过GPU来测试训练能否正常执行。在本地有GPU的情况下可以使用该命令测试训练代码能否正常执行。（注：在使用前请确认GPU驱动已经安装，并已经安装了nvidia-docker，详细安装方法请参见[[ai:uai-train:basic:docker]]）

### 本地镜像输出
在本地镜像仓库可以看到生成了两个docker镜像，分别为cpu版本和gpu版本。如下：
<code>
$ sudo docker images
REPOSITORY						  TAG		IMAGE ID	CREATED		SIZE
tf-mnist-cpu						uaitrain	xxxxxx		xxxx ago	xxx GB
uhub.ucloud.cn/<YOUR_UHUB_REFDISTRY>/tf-mnist	uaitrain	xxxxxx		xxxx ago	xxx GB
</code>

### 标准输出文件
本地文件夹下生成了uaitrain\_cmd.txt以及相关的日志文件，其中uaitrain\_cmd.txt内容和**标准输出**的内容一致，防止用户丢失屏幕输出内容。

## Step5: 自定义软件包安装## 
如果训练代码依赖特殊的软件包，例如nltk 等，可以通过Docker 打包的形式将软件包和相关数据打包入训练的Docker镜像，详细方法参见[[ai:uai-train:guide:tensorflow:userpack]]

