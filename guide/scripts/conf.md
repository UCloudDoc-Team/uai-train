{{indexmenu_n>11}}



# conf-获取任务配置
使用该命令可以查询训练任务的详细信息，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py conf --args

python tf/tf_tool.py conf --args
python caffe/caffe_tool.py conf --args
python keras/keras_tool.py conf --args
python mxnet/mxnet_tool.py conf --args
python pytorch/pytorch_tool.py conf --args
</code>

## 命令参数说明
| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key         | 用户的公钥                                              | 是              |        无     |
| private\_key        | 用户的私钥                                              | 是              |        无     |
| job\_id                | 任务ID（Create 命令的返回值）                 | 否              |      无      |
| project\_id         | 项目组id                                                  | 否               |        用户默认项目组   |
| region               | 训练任务执行所在地域                                 | 否               |       默认地域（北京二）   |
| zone                 | 训练任务所在可用区                                    | 否              |        默认可用区   |

## 返回值说明
**返回任务的配置信息**，具体的输出如下：
JOB\_NAME: 任务名字; JOB\_ID: 任务ID; CODE\_IMAGE: Uhub镜像地址; DATA\_PATH: 数据输入路径; OUT\_PATH:输出路径; CMD:启动命令; STATUS: 任务状态; EXEC\_TIME: 任务执行时间（秒）; PRICE: 该任务已用费用（元）

## 命令操作案例
查询某任务的配置信息
<code>
python tf_tool.py conf --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --job_id=’<JOB_ID>’
</code>