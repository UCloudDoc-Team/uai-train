{{indexmenu_n>2}}

=====开发综述  =====
面向UAI Train 开发训练代码、Docker镜像和数据的基本原则如下： \\
  - 选择GPU节点的训练任务，使用基于nvidia-docker的镜像，内部需要内置cuda、cudnn等库，UCloud已经提供丰富的GPU基础镜像选择，客户可以自由选择
  - **训练的代码请封装在Docker镜像中**
  - **训练代码的入口必须为python程序，系统会默认以/usr/bin/python 执行python程序，默认python 执行路径为/data/**
  - 训练默认会增加如下参数： \-\-num\_gpus=<num> \-\-data\_dir=/data/data \-\-output\_dir=/data/output/ \-\-work\_dir=/data/
  - 数据后端存储会使用UCloud的公共存储服务（UFile、UFS）等
  - **输入数据默认会下载到Docker 容器中的/data/data/ 目录下，目录结构保存**
  - **输出数据需要默认放在Docker 容器的/data/output/ 目录下，数据会在训练完成后自动上传**
  - 训练使用的节点**无外网访问能力**

===UAI Train Docker镜像准备===
用户可以根据如下指南开发对应AI框架的训练代码+容器：\\
[[ai:uai-train:guide:tensorflow]] \\
[[ai:uai-train:guide:caffe]] \\
[[ai:uai-train:guide:keras]] \\
[[ai:uai-train:guide:mxnet]] \\
[[ai:uai-train:guide:pytorch]] \\

===自定义Docker 镜像打包工具===
用户可以根据以上准则开发自定义的 **训练容器**
[[:ai:uai-train:guide:scripts:self-pack]] \\
用户可以在**免费资源/基础Docker镜像**中找到安装了基础环境的Docker镜像
[[:ai:uai-train:resource:docker]] \\