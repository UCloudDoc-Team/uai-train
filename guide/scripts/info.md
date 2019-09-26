{{indexmenu_n>4}}

# 训练任务信息（Info）
使用该命令可以查询训练任务的简单状态，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py info --args

python tf/tf_tool.py info --args
python caffe/caffe_tool.py info --args
python keras/keras_tool.py info --args
python mxnet/mxnet_tool.py info --args
python pytorch/pytorch_tool.py info --args
</code>

## 命令参数说明
| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key         | 用户的公钥                                              | 是              |        无     |
| private\_key        | 用户的私钥                                              | 是              |        无     |
| job\_id                | 任务ID（Create 命令的返回值）                 | 是              |      无      |
| project\_id         | 项目组id                                                  | 否               |        用户默认项目组   |
| region               | 训练任务执行所在地域                                 | 否               |       默认地域（北京二）   |
| zone                 | 训练任务所在可用区                                    | 否              |        默认可用区   |

## 返回值说明
**返回值显示任务相关的执行和计费信息**，返回格式如下：
JOB\_ID: 任务ID; ExecTime: 执行时间（秒）; Total Cost: 总开销（元，精确到0.01元）

## 命令操作案例
<code>
python tf_tool.py info --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --job_id=’<JOB_ID>’
</code>