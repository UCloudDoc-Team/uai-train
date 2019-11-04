

# MXNet分布式训练
UAI Train MXNet的分布式训练环境实现基于 MXNet的分布式训练系统实现。PS和Worker采用混合部署的方式部署，PS使用纯CPU计算，Worker使用GPU+CPU计算。部署方式参见[[ai:uai-train:introduction:distributed-job:intro]]。

## MXNet 分布式训练简介
MXNet 分布式训练基于DMLC分布式机器学习框架来运行实现。在参数中指定kv_store为分布式形式(dist_sync或dist_async)后，MXNet系统会自动选择DMLC进行分布式训练。DMLC 执行分布式训练时包括三个部分：

  * scheduler，调度器，负责调度Parameter Server和Worker
  * server，Parameter Server，作为参数服务器
  * worker，Worker Server，作为计算节点

DMLC的kvstore系统通过一系列**DMLC**环境变量来配置分布式训练环境，其包括5个环境变量：
  * DMLC\_NUM\_WORKER: 一共多少个worker
  * DMLC\_NUM\_SERVER: 一共多少个PS
  * DMLC\_ROLE`: 当前节点的角色,可以是worker, server, scheduler中任意一个
  * DMLC\_PS\_ROOT\_URI: scheduler 节点的URI地址
  * DMLC\_PS\_ROOT\_PORT: scheduler节点监听的端口
worker和ps server根据环境变量配置来向scheduler注册自己，并获取分布式训练的拓扑信息。scheduler在获取所有worker和ps server的信息之后就会发起训练任务，并同步控制流信息。

## UAI Train 分布式训练简介
UAI Train系统将自动为分布式训练任务生成DMLC环境变量，系统在启动时会先创建scheduler容器进程，然后再创建ps server容器进程和worker容器进程，UAI Train 分布式训练系统在执行MXNet分布式任务时有如下约定：

  * Scheduler、PS server 和 Worker Server的执行逻辑均使用Docker容器封装，且**Scheduler、PS Server和Worker Server使用相同的Docker镜像执行**。
  * Parameter Server 和 Worker Server **代码执行逻辑的入口相同**，MXNet的分布式训练系统会自行根据DMLC的环境变量执行对应逻辑。
  * **UAI Train 系统将自动为分布式任务的Scheduler、PS和Worker节点生成DMLC配置信息。**
  * 在UAI Train系统完成环境准备（包括拉取代码镜像和数据加载）后，系统将自动执行分布式训练。
  * 当所有的Worker完成任务并自行退出后，系统将自动清理所有资源。
  * Parameter Server 和 Worker Server将使用共享分布式文件系统UFS来保持数据，您需要将**模型的输出路径指向/data/output目录**，该目录为分布式系统的默认输出目录。
  * UAI Train系统会分别将所有的Parameter Server和Worker Server的标准输出推送至图形界面，同时也会保存在您所指定的UFS存储中。

UAI Train系统在执行MXNet分布式训练scheduler、ps server和worker server的部署方式如下：

{{:ai:uai-train:dist:uai-mxnet-dist.png?400|}}

**注意事项：**
请不要改变PS_VERBOSE=1环境变量参数，这会导致分布式训练无法刷新日志并导致分布式训练系统无法执行。

