{{indexmenu_n>4}}

===== 打包镜像=====
UAI-Train为用户提供了镜像打包工具，用户只需将所需代码文件放在某一路径下，执行打包命令即可以生成UAI-Train所需的镜像。\\

该打包工具将在本地docker中生成**两个镜像**以及**运行镜像的指令说明文件uaitrain_cmd.txt**。生成的镜像包括cpu和gpu两个版本，其中gpu版本的镜像会自动上传至用户的Uhub镜像仓库。两个版本的镜像均可以用于本地测试，测试命令可在uaitrain\_cmd.txt中查询。\\

===Step0: 安装最新版本的UAI SDK和docker支持===
安装UAI SDK的方法如下：
<code>
git clone https://github.com/ucloud/uai-sdk
cd uai-sdk
sudo python setup.py install
</code>

安装Docker的方法请参见：[[ai:uai-train:base:docker]]

===Step1: 找到UAI-Train Caffe操作工具所在目录===
<code>
$ls ~/uai-sdk/uaitrain_tool/caffe
caffe_tool.py
</code>

===Step2: 将AI训练任务所需的代码放在统一路径下，打包时将其相对路径作为参数code_path上传===
例如，我们要将~/uai\-sdk/examples/caffe/train/mnist下面的训练代码进行打包，该文件路径结构如下：
<code>
$ cd ~/uai-sdk/examples/caffe/train/mnist
$ ls 
data  code
</code>
我们需要做如下准备工作：
  - 准备好训练的代码，案例中训练代码在mnist/code下, 包括train.py，lenet\_solver.prototxt 和 lenet\_train\_test.prototxt，其中lenet\_solver.prototxt为sovler描述，lenet\_train\_test.prototxt为模型描述文件。
  - 将caffe\_tool.py 工具放入和训练代码目录同级的目录下，即mnist 目录下
  - Ready To Pack
<code>
$ cd ~/uai-sdk/examples/caffe/train/mnist
$ ls
data/ code/

$ cp ~/uai-sdk/uaitrain_tool/caffe/caffe_tool.py ./
$ ls
data/ code/ caffe_tool.py 
</code>

===Step3: 执行pack命令，完成镜像的打包===
caffe\_tool.py pack命令执行方法如下：
<code>
sudo python caffe_tool.py pack [-h] --public_key PUBLIC_KEY 
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
                        [--train_params TRAIN_PARAMS]
                        --solver PATH_TO_SOLVER
                        [--snapshot PATH_TO_SNAPSHOT]
</code>

^ 参数                  ^                                                                                                     说明 ^ 是否必需           ^
| public\_key         | 用户的公钥                                                                                                  | 是              |
| private\_key        | 用户的私钥                                                                                                  | 是              |
| project\_id         | 项目id                                                                                                   | 否              |
| code\_path          | 训练任务所需代码路，以/结尾(请填写相对caffe_tool.py的相对路径，本例中为./code/)                                                    | 是              |
| mainfile\_path      | 训练主文件（有main函数），UAI 为Caffe框架提供了train.py 入口代码，可以直接填写train.py，也可以自定义                                      | 是              |
| uhub\_username      | uhub账号(即UCloud账号)                                                                                      | 是              |
| uhub\_password      | uhub密码(即UCloud账号密码)                                                                                    | 是              |
| uhub\_registry      | uhub镜像仓库名称，不支持大写字母                                                                                     | 是              |
| uhub\_imagename     | 生成镜像名称，不支持大写字母                                                                                         | 是              |
| uhub\_imagetag      | 生成镜像tag，不支持大写字母                                                                                        | 否，默认为uaitrain  |
| internal\_uhub      | 告诉打包程序是否使用uhub的UCloud内网地址，如果true则使用uhub.service.ucloud.cn，false则使用uhub.ucloud.cn，使用内网地址可以提高镜像拉取和上传的速度  | 否，默认为false            |
| ai\_arch\_v         | 深度学习框架以及版本，目前仅支持 caffe-1.0.0                                                                           | 是              |
| test\_data\_path    | 本地测试环境数据输入路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| test\_output\_path  | 本地测试环境训练输出路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| train\_params       | 训练所需参数，对于Caffe来说，训练参数通常由solver文件定义，因此可以忽略该参数                                                           | 否              |
| solver              | Caffe 的 Solver proto 定义文件的路径                                                                           | 是              |
| snapshot            | Caffe 训练时用于restore的snapshot文件的路径                                                                       | 否              |

**solver指定了Solver proto的路径，打包工具会自动生成\-\-solver=<solver>的语句**, 通常有两种填写方法：
  - /data/xxx，此时solver proto文件需要和train.py 一同打包进/data/ 目录下
  - /data/data/xxx，此时solver proto文件需要和数据一同上传至UFIle存储中，训练过程文件将下载至/data/data/目录下。**用户可以通过该方案，在不重上传Docker镜像的基础上，动态修改solver proto 文件**

=== 可以选镜像及相关命令组合 ===
^ Caffe              ^ 操作系统              ^ Python         ^ 命令组合                                                                                      ^
| caffe-1.0.0 + py2  | ubuntu\-14.04.05  | python\-2.7.6  | \-\-ai\_arch\_v=caffe-1.0.0|                                                  
**命令样例**\\
使用mnist中的训练程序为案例。\\
test\_data\_path和test\_data\_path不要求一定在训练代码路径下，如我们可以在/data/test目录下创建了两个子目录：
  * /data/test/data 用于存放训练数据，此时test\_data\_path值为/data/test/data
  * /data/test/output 用于存放训练输出数据，此时test\_output\_path为/data/test/output 
使用命令时，需要使用sudo，保证docker镜像打包命令有足够权限。

注：我们可以将~/uai\-sdk/examples/caffe/train/mnist/data/ 下的测试数据放入/data/test/data/目录下 \\
同时将~/uai\-sdk/examples/caffe/train/mnist/code/ 下的lenet\_solver.prototxt 和 lenet\_train\_test.prototxt 文件也放入/data/test/data/目录下

<code>
$ ls /data/test/data/
mnist_test_lmdb/ mnist_train_lmdb/ lenet_solver.prototxt lenet_train_test.prototxt
$ cd ~/uai-sdk/examples/caffe/train/mnist
$ ls
data/ code/ caffe_tool.py 
$ sudo python caffe_tool.py pack \
                        --public_key=<YOUR_PUBLIC_KEY> \
			--private_key=<YOUR_PRIVATE_KEY> \
			--code_path=./code/ \
			--mainfile_path=train.py \
			--uhub_username=<YOUR_UHUB_USER_NAME> \
			--uhub_password=<YOUR_UHUB_PASSWORD> \
			--uhub_registry=<YOUR_UHUB_REFDISTRY> \
			--uhub_imagename=<YOUR_UHUB_IMAGENAME> \
                        --internal_uhub=true \
			--ai_arch_v=caffe-1.0.0 \
			--test_data_path=/data/test/data \
			--test_output_path=/data/test/output \
			--solver=/data/data/lenet_solver.prototxt
</code>
执行完指令后，训练镜像就已经准备好了

===Step4: 输出说明===
==标准输出==
成功执行后，界面显示样例如下，会给出部署时所需的CMD命令以及本地测试的cmd命令:
<code>
CMD Used for deploying:
 /data/train.py --solver=/data/data/lenet_solver.prototxt
CMD for CPU local test:
sudo docker run -it -v /data/test/data:/data/data -v /data/test/output:/data/output caffe-mnist-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/train.py --solver=/data/data/lenet_solver.prototxt --use_cpu=True --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log" 
CMD for GPU local test:
sudo nvidia-docker run -it -v /data/test/data:/data/data -v /data/test/output:/data/output uhub.service.ucloud.cn/caffe-demo/caffe-mnist:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/train.py --solver=/data/data/lenet_solver.prototxt --use_cpu=False --num_gpus=1 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>
  * **CMD Used for deploying**: 该输出的内容为创建训练任务时，**训练启动命令**框中需要填写的内容(参见[[ai:uai-train:use:create]])。可以直接复制黏贴到命令框中。
  * **CMD for CPU local test**: 该输出的内容为本地通过CPU来测试训练能否正常执行。在本地没有GPU的情况下可以使用该命令测试训练代码能否正常执行。
  * **CMD for GPU local test**：该输出的内容为本地通过GPU来测试训练能否正常执行。在本地有GPU的情况下可以使用该命令测试训练代码能否正常执行。（注：在使用前请确认GPU驱动已经安装，并已经安装了nvidia-docker，详细安装方法请参见[[ai:uai-train:base:docker]]）

==本地镜像输出==
在本地镜像仓库可以看到生成了两个docker镜像，分别为cpu版本和gpu版本。如下：
<code>
$ sudo docker images
REPOSITORY						  TAG		IMAGE ID	CREATED		SIZE
caffe-mnist-cpu						uaitrain	xxxxxx		xxxx ago	xxx GB
uhub.ucloud.cn/<YOUR_UHUB_REFDISTRY>/caffe-mnist	uaitrain	xxxxxx		xxxx ago	xxx GB
</code>

==标准输出文件==
本地文件夹下生成了uaitrain\_cmd.txt以及相关的日志文件，其中uaitrain\_cmd.txt内容和**标准输出**的内容一致，防止用户丢失屏幕输出内容。

===Step5: 自定义软件包安装===
如果训练代码依赖特殊的软件包，例如nltk 等，可以通过Docker 打包的形式将软件包和相关数据打包入训练的Docker镜像，详细方法参见[[ai:uai-train:guide:caffe:userpack]]

