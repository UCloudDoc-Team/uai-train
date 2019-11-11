

# TensorFlow分布式训练
UAI Train Tensorflow的分布式训练环境实现基于TensorFlow 的分布式训练系统实现，采用默认的grpc协议进行数据交换。PS和Worker采用混合部署的方式部署，PS使用纯CPU计算，Worker使用GPU+CPU计算。部署方式[参见](ai/uai-train/introduction/distributed-job/intro)。

## TensorFlow 分布式训练简介
TensorFlow 分布式训练采用PS-Worker的分布式格式，并提供python的接口运行分布式训练。

  * 如果您的代码使用Tensorflow Estimators[接口实现](https://www.tensorflow.org/programmers_guide/estimators)，该接口可以直接运用于分布式训练系统。
  * 如果您采用tf.train.Server接口，您需要自行实现启动Parameter Server和Worker Server的[逻辑](https://www.tensorflow.org/deploy/distributed)

### Estimators 接口
Estimators（tf.estimator.Estimator）是TensorFlow提供的高级API接口，该接口可用于将训练（training）、评估（evaluation）、推理（prediction）和服务化（export for serving）的逻辑包装在一起。使用Estimators API实现的代码既可以在单机运行，也可以在分布式环境中运行。在分布式环境中运行时，我们只需要在环境变量中配置好整个分布式训练的网络即可。整个配置方法如下：
<code>
 cluster = {"master": ["master-ip:port"],
            "ps": ["ps-ip0:port", "ps-ip1:port", "ps-ip2:port"],
            "worker": ["worker-ip0:port", "worker-ip1:port"]}

task = {"type":"worker", "index":0}

 TF_CONFIG = {"cluster": cluster,
    	       "task": task,
               "environment": "cloud"}
</code>
在设置好TF_CONFIG环境变量后，Estimators接口会自动解析里面的分布式集群配置格式，并执行分布式训练任务。

**使用Estimators接口实现的训练代码在UAI Train平台上可以使用相同的代码Docker和启动命令运行单节点训练和分布式训练**

### tf.train.Server 接口
tf.train.Server接口是TensorFlow提供的分布式训练接口，详细说明可以[参见](https://www.tensorflow.org/deploy/distributed)。

  - 在实现分布式训练代码时，我们需要定义一个 tf.train.ClusterSpec 对象，该对象描述了PS和Worker的拓扑信息。
  - 然后使用tf.train.Server接口创建grpc Server，在创建Server时，我们需要指定该Server的job\_name（ps或者worker）以及其在网络拓扑中的index。
  - 在创建tf.Session的时候，需要指定该Session所执行的Server的地址。

同样的我们也可以通过TF\_CONFIG环境变量来传递分布式训练的拓扑信息。然后通过
<code>
	env = os.environ['TF_CONFIG']
</code>
获取TF\_CONFIG 的值，并解析成分布式训练Cluster的配置信息。

## UAI Train 分布式训练简介
UAI Train 分布式训练系统在执行分布式任务时有如下约定：

  * Parameter Server 和 Worker Server的执行逻辑均适用Docker容器封装，且**PS和Worker使用相同的Docker镜像执行**
  * Parameter Server 和 Worker Server **代码执行逻辑的入口相同**，您需要根据TF\_CONFIG环境变量来区分当前容器的角色(PS/Worker)和编号(index)
  * UAI Train 系统将自动为分布式任务的PS和Worker节点生成TF\_CONFIG配置信息。
  * 在UAI Train系统完成环境准备（包括拉取代码镜像和数据加载）后，系统将自动执行分布式训练。
  * 当所有的Worker完成任务并自行退出后，系统将自动清理所有资源。
  * Parameter Server 和 Worker Server将使用共享分布式文件系统UFS来共享数据，您需要将**模型的输出路径和TensorBoard的输出路径指向/data/output目录**，该目录为分布式系统的默认输出目录。
  * UAI Train系统会分别将所有的Parameter Server和Worker Server的标准输出推送至图形界面，同时也会保存在您所指定的UFS存储中。

### 自动生成TF_CONFIG的逻辑
UAI Train系统将自动为分布式训练任务生成TF\_CONFIG环境变量，假设我们有执行一个4节点分布式训练，IP分别为 ip0, ip1, ip2, ip3，系统自动生成网络拓扑信息**xluster**:
<code>
 cluster = {"master": ["ip0:port-worker"],

​            "ps": ["ip0:port-ps", "ip1:port-ps", "ip2:port-ps", "ip3:port-ps"],
​            "worker": ["ip1:port-worker", "ip2:port-worker", "ip3:port-worker"]}
</code>
其中第一个IP ip0为设置为master，其他为worker。使用**Estimators** API，系统将自动识别该拓扑信息。 如果使用**tf.train.Server**，请自行解析该拓扑信息，并将master对应的ip:port 设定为**chief** 节点。

系统会为每一个节点生成任务信息，例如ip0对应的节点会生成两个任务类型：
<code>
#PS 任务
TF_CONFIG = {"cluster": cluster,
    	       "task":  {"type":"ps", "index":0},
               "environment": "cloud"}

#Worker 任务
TF_CONFIG = {"cluster": cluster,
    	       "task":  {"type":"worker", "index":0},
               "environment": "cloud"}
</code>
其他节点以此类推，使用**Estimators** API，系统将自动识别该节点类型信息。 如果使用**tf.train.Server**，请自行解析该节点类型信息，其中type=ps代表Parameter Server，type=worker代码Worker。

