

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

安装Docker的方法请参见：[](ai/uai-train/basic/docker)

### Step1: 找到UAI-Train pytorch 操作工具所在目录
<code>
$ls ~/uai-sdk/uaitrain_tool/pytorch
pytorch_tool.py
</code>

### Step2: 将AI训练任务所需的代码放在统一路径下，打包时将其相对路径作为参数code_path上
例如，我们要将~/uai\-sdk/examples/pytorch/train/mnist 下面的训练代码进行打包，该文件路径结构如下：
<code>
$ cd ~/uai-sdk/examples/pytorch/train/mnist

$ ls
data  code
</code>
我们需要做如下准备工作：

  - 准备好训练的代码，案例中训练代码在mnist/code下, 名为 **mnist.py**
  - 将pytorch\_tool.py 工具放入和训练代码目录同级的目录下，即mnist/目录下
  - Ready To Pack

<code>
$ cd ~/uai-sdk/examples/pytorch/train/mnist
$ ls
data/ code/

$ cp ~/uai-sdk/uaitrain_tool/pytorch/pytorch_tool.py ./
$ ls
data/ code/ pytorch_tool.py 
</code>

### Step3: 执行pack命令，完成镜像的打包
pytorch\_tool.py pack命令执行方法如下：
<code>
sudo python pytorch_tool.py pack [-h] --public_key PUBLIC_KEY

​                        --private_key PRIVATE_KEY 
​                        [--project_id PROJECT_ID] 
​                        --code_path CODE_PATH 
​                        --mainfile_path MAINFILE_PATH
​                        --uhub_username UHUB_USERNAME
​                        --uhub_password UHUB_PASSWORD 
​                        --uhub_registry UHUB_REGISTRY
​                        --uhub_imagename UHUB_IMAGENAME
​                        [--uhub_imagetag UHUB_IMAGETAG]
​                        [--internal_uhub false/true]
​                        --ai_arch_v AI_ARCH_V
​                        --test_data_path TEST_DATA_PATH
​                        --test_output_path TEST_OUTPUT_PATH
​                        --train_params TRAIN_PARAMS
</code>

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| public\_key         | 用户的公钥                                                                                                  | 是              |
| private\_key        | 用户的私钥                                                                                                  | 是              |
| project\_id         | 项目id                                                                                                   | 否              |
| code\_path          | 训练任务所需代码路，以/结尾(请填写相对pytorch_tool.py的相对路径，本例中为./code/)                                                  | 是              |
| mainfile\_path      | 训练主文件（有main函数），不须包含路径，但须包含后缀名（本例中为：mnist.py）                                                           | 是              |
| uhub\_username      | uhub账号(即UCloud账号)                                                                                      | 是              |
| uhub\_password      | uhub密码(即UCloud账号密码)                                                                                    | 是              |
| uhub\_registry      | uhub镜像仓库名称，不支持大写字母                                                                                     | 是              |
| uhub\_imagename     | 生成镜像名称，不支持大写字母                                                                                         | 是              |
| uhub\_imagetag      | 生成镜像tag，不支持大写字母                                                                                        | 否，默认为uaitrain  |
| internal\_uhub      | 告诉打包程序是否使用uhub的UCloud内网地址，如果true则使用uhub.service.ucloud.cn，false则使用uhub.ucloud.cn，使用内网地址可以提高镜像拉取和上传的速度  | 否，默认为false     |
| ai\_arch\_v         | 深度学习框架以及版本，暂仅支持pytorch-0.2.0                                                                           | 是              |
| test\_data\_path    | 本地测试环境数据输入路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| test\_output\_path  | 本地测试环境训练输出路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| train\_params       | 训练所需参数                                                                                                 | 否              |
| os                  | 操作系统名称以及版本，用"-"分隔，形如"ubuntu-14.04.05"                                                                  | 否，默认ubuntu-14.04.05  |
| python\_version     | python名称以及版本，用"-"分隔，形如"python-2.7.6"                                                                   | 否，默认python-2.7.6     |

### 可以选镜像及相关命令组合
| Pytorch | 操作系统 | Python | 命令组合 |
| ------- | -------- | ------ | -------- |
| pytorch-0.4 + py3    | ubuntu-16.04      | python-3.6.2   | \-\-ai\_arch\_v=pytorch-0.4 \-\-python\_version=python-3.6.2 \-\-os=ubuntu-16.04  |
| pytorch-0.2.0 + py2  | ubuntu\-14.04.05  | python\-2.7.6  | \-\-ai\_arch\_v=pytorch-0.2.0                                                     |

**命令样例**
在mnist案例中，输入数据存放在uai-sdk的examples/pytorch/train/mnist/data目录下 
test\_data\_path为/home/ubuntu/uai-sdk/examples/pytorch/train/mnist/data 
test\_output\_path为/home/ubuntu/uai-sdk/examples/pytorch/train/mnist/output 
使用命令时，需要使用sudo，保证docker镜像打包命令有足够权限。
<code>
$ cd ~/uai-sdk/examples/pytorch/train/mnist

$ ls
data/ code/ pytorch_tool.py 
$ sudo python pytorch_tool.py pack \
--public_key xxxx \
--private_key xxxx \
--code_path ./code/ \
--mainfile_path mnist.py \
--uhub_username xxxx  \
--uhub_password xxxx  \
--uhub_registry xxxx  \
--uhub_imagename pytorch-mnist \
--test_data_path /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/data \
--test_output_path /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/output \
--ai_arch_v pytorch-0.2.0 
</code>
执行完指令后，训练镜像就已经准备好了

### Step4: 输出说明
#### 标准输出
成功执行后，界面显示样例如下，会给出部署时所需的CMD命令以及本地测试的cmd命令:
<code>
CMD Used for deploying: /data/mnist.py

CMD for CPU local test: sudo docker run -it -v /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/data:/data/data -v /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/output:/data/output pytorch-mnist-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist.py  --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
CMD for GPU local test: sudo nvidia-docker run -it -v /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/data:/data/data -v /home/ubuntu/uai-sdk/examples/pytorch/train/mnist/output:/data/output uhub.service.ucloud.cn/fanrongtest/pytorch-mnist:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist.py  --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output"
</code>

  * **CMD Used for deploying**: 该输出的内容为创建训练任务时，**训练启动命令**框中需要填写的内容参见[](ai/uai-train/guide/scripts/create)。可以直接复制黏贴到命令框中。
  * **CMD for CPU local test**: 该输出的内容为本地通过CPU来测试训练能否正常执行。在本地没有GPU的情况下可以使用该命令测试训练代码能否正常执行。
  * **CMD for GPU local test**：该输出的内容为本地通过GPU来测试训练能否正常执行。在本地有GPU的情况下可以使用该命令测试训练代码能否正常执行。（注：在使用前请确认GPU驱动已经安装，并已经安装了nvidia-docker，详细安装方法请参见[](ai/uai-train/basic/docker)）

#### 本地镜像输出
在本地镜像仓库可以看到生成了两个docker镜像，分别为cpu版本和gpu版本。如下：
<code>
$ sudo docker images

REPOSITORY						  TAG		IMAGE ID	CREATED		SIZE
pytorch-mnist-cpu						uaitrain	xxxxxx		xxxx ago	xxx GB
uhub.ucloud.cn/<YOUR_UHUB_REFDISTRY>/pytorch-mnist	uaitrain	xxxxxx		xxxx ago	xxx GB
</code>

#### 标准输出文件
本地文件夹下生成了uaitrain\_cmd.txt以及相关的日志文件，其中uaitrain\_cmd.txt内容和**标准输出**的内容一致，防止用户丢失屏幕输出内容。

### Step5: 自定义软件包安装
如果训练代码依赖特殊的软件包，例如nltk 等，可以通过Docker命令将软件包放至Docker容器中，然后通过docker commit将其保存为镜像即可。
**注：输入数据无需存储在容器中，可上传至UFile**

