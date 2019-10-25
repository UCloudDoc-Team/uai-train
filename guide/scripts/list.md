

# list-查询任务详细信息
使用该命令可以查询训练任务的详细信息，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py list --args

python tf/tf_tool.py list --args
python caffe/caffe_tool.py list --args
python keras/keras_tool.py list --args
python mxnet/mxnet_tool.py list --args
python pytorch/pytorch_tool.py list --args
</code>

## 命令参数说明
| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key         | 用户的公钥                                              | 是              |        无     |
| private\_key        | 用户的私钥                                              | 是              |        无     |
| job\_id                | 任务ID（Create 命令的返回值）                 | 否              |      无      |
| limit                   | 最大显示训练任务数量                               | 否              |      0，默认返回所有任务信息      |
| offset                   | 首条任务信息的相对偏移量，在limit非0时有效                           | 否              |      0     |
| project\_id         | 项目组id                                                  | 否               |        用户默认项目组   |
| region               | 训练任务执行所在地域                                 | 否               |        默认地域（北京二）   |
| zone                 | 训练任务所在可用区                                    | 否              |        默认可用区   |

### 查询单个任务信息
**--job\_id--填写不为空，此时会查询该单个任务运行状态**

### 查询多个任务信息
**--job\_id--为空，则查询<limit>条任务信息（如果查询批量任务，建议使用UCloud console的图形界面）**

## 返回值说明
**返回值会罗列所有返回任务的执行信息**，具体的输出如下：
JOB\_NAME: 任务名字; JOB\_ID: 任务ID; BUSINESS\_ID: 任务业务组; STATUS: 任务当前状态; CREATE\_TIME:任务创建时间; START\_TIME:任务开始时间; END\_TIME: 任务结束时间(如果任务正在运行，该值请忽略)

## 命令操作案例
查询当个任务信息
<code>
python tf_tool.py list --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --job_id=’<JOB_ID>’
</code>

查询最近20条任务信息
<code>
python tf_tool.py list --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --limit=20
</code>

