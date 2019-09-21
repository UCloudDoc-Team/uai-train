{{indexmenu_n>1}}

# 分布式训练简介
UCloud AI Train分布式训练采用Parameter Server和Worker Server混部的方法，所有计算节点均由GPU物理云主机组成。PS 仅使用CPU进行计算，Worker Server则同时使用GPU和CPU进行计算，PS 和 Worker的比例为1:1。

{{:ai:uai-train:dist:uai-dist.png?600|}}

## UAI Train 分布式训练的存储
分布式训练所使用的输入数据和输入数据可以是来自不同的数据源，目前UAI Train仅支持UFS作为数据的存储。您可以在[[ai:uai-train:basic:ufs]]了解UFS的简单使用方法。

### Input 数据存储
您可以任意指定一个UFS盘作为Input数据源，UAI Train平台在训练执行过程中会将对应的UFS数据映射到训练执行的Worker容器的 /data/data 目录下，假设您的UFS盘为 ip:/xxx，您需要使用里面/data/imagenet/tf/ 下的数据作为训练的输入数据，则系统会自动将数据映射到执行的容器中，如 ip:/xxx/data/imagenet/tf -> /data/data/

### Output 数据存储
您可以任意指定一个UFS盘作为output数据源，UAI Train平台在训练执行过程中会将对应的UFS数据映射到训练执行的每一个PS容器和Worker容器的 /data/output 目录下，并以共享的方式访问同一份数据。假设您的UFS盘为 ip:/yyy，您希望将训练输出数据保存在/output/目录下，则系统会自动将输出路径映射到执行的容器中，如 ip:/yyy/output -> /data/data/。同时，在训练过程您可以通过其他云主机实时访问训练保持的模型checkpoint。

## UAI Train 分布式训练代码
您需要自己准备分布式训练的代码，目前UAI Train平台支持TensorFLow 和 MXNet 框架的分布式训练。您需要将PS的代码和Worker的代码实现在同一个代码入口中，在执行过程中，PS 和 Worker 将使用相同的Docker容器镜像和相同的python代码入口进行执行，系统将自动生成PS和Worker的env环境参数。详细说明请参考[[ai:uai-train:introduction:distructive-job:tensorflow]]和[[ai:uai-train:introduction:distructive-job:mxnet]]

