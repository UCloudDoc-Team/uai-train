

# 交互式训练
AI 交互式训练服务是面向AI训练任务的交互式开发环境，基于Jupyter的交互式变成界面，为用户提供开箱即用的AI开发环境。同时依托UCloud强大算力支撑，提供GPU云主机为主的高性能计算节点。用户可以使用UCloud提供的默认Docker镜像启动训练任务（其中已包含，tensorflow,caffe,mxnet,keras训练环境），并在此基础上加载通过UFS，UDisk加载自己的云端数据，通过外网服务能力个性化定制环境依赖包。用户可以随时启停交互式开发环境，系统按需环境启动时间收费，避免不必要的资源浪费。系统提供了镜像保存功能，以便用户随时保存开发环境的最新版本。
![](ai/uai-train/images/intro/ai交互式训练任务操作流程.png)

AI交互式训练任务过程一共分三大部分，一共九个步骤：

### 训练准备  

（非必须）使用控制台页面或SDK工具将训练用数据上传到云存储产品（如US3，UFS）指定路径；  
（非必须）通过交互式训练的保存镜像功能制作自定义镜像。  

### 训练任务执行  

通过UAI Train平台操作界面发起AI交互式训练任务；  
（非必须）使用Terminal界面自主安装研发需要的外部依赖包，保存镜像生成自定义研发环境镜像；  
运用Jupyter Notebook实现实时开发；  
运用Jupyter Notebook运行调试训练程序生成模型。  

### 训练完成  

（非必须）通过保存镜像按钮保存当前开发环境到Uhub Docker进行仓库；  
停止交互式训练任务；  
使用控制台页面或SDK工具从云存储产品（如US3，UFS）获取训练结果。  

## UAI Train 交互式训练的存储
交互式训练有两种数据存储方式,两种存储均已容器的/data路径为Jupyter网页中的/路径：

1. 镜像存储：
	* 使用容器镜像本身的存储容量，容量有限，无法扩展。  
	* 可打包在镜像中。 
	* 一般用来存储用户对镜像环境的升级更新。
2. 外部存储：
	* 可以是来自不同的数据源，此种存储容量大、易扩展。
	* 外部存储不能被打包到镜像中，但是可以每次启动是通过挂载相同外部存储数据源地址做到数据更新。
	* 一般用来存储用户的训练数据、训练结果等。
	* 目前UAI Train仅支持UFS作为数据的存储。您可以在[](uai-train/basic/ufs)了解UFS的简单使用方法。

### Code 数据存储
您可以任意指定一个UFS盘作为Code数据源，UAI Train平台在训练执行过程中会将对应的UFS数据映射到训练执行的Jupyter容器的 /data 目录下（Jupyter网页中的/目录），假设您的UFS盘为 ip:/xxx，您需要使用里面/data/imagenet/tf/ 下的数据作为训练的输入数据，则系统会自动将数据映射到执行的容器中，如 ip:/xxx/data/imagenet/tf -> /data（Jupyter网页中的/目录）

### Input 数据存储
您可以任意指定一个UFS盘作为Input数据源，UAI Train平台在训练执行过程中会将对应的UFS数据映射到训练执行的Jupyter容器的 /data/data 目录下（Jupyter网页中的/data目录），假设您的UFS盘为 ip:/xxx，您需要使用里面/data/imagenet/tf/ 下的数据作为训练的输入数据，则系统会自动将数据映射到执行的容器中，如 ip:/xxx/data/imagenet/tf -> /data/data/（Jupyter网页中的/data目录）

### Output 数据存储
您可以任意指定一个UFS盘作为output数据源，UAI Train平台在训练执行过程中会将对应的UFS数据映射到训练执行的Jupyter容器的 /data/output 目录下（Jupyter网页中的/output目录），并以共享的方式访问同一份数据。假设您的UFS盘为 ip:/yyy，您希望将训练输出数据保存在/output/目录下，则系统会自动将输出路径映射到执行的容器中，如 ip:/yyy/output -> /data/output/（Jupyter网页中的/output目录）。

## UAI Train 交互式训练镜像
初次启动采用系统默认镜像，可在此基础上完成升级更新后，通过“保存镜像”功能保存自己定制的Jupyter镜像版本。

## UAI Train 交互式训练常见操作
[createinterjob](uai-train/guide/console/createinterjob)

[jupyter](uai-train/guide/console/jupyter)

[saveinterjobimg](uai-train/guide/console/saveinterjobimg)

[stopinterjob](uai-train/guide/console/stopinterjob)

[startinterjob](uai-train/guide/console/startinterjob)

[deleteinterjob](uai-train/guide/console/deleteinterjob)

