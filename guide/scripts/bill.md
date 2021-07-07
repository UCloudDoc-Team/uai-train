



# bill-计费信息
使用该命令可以查询指定时间段内的全部任务的计费信息，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py bill --args

python tf/tf_tool.py bill --args
python caffe/caffe_tool.py bill --args
python keras/keras_tool.py bill --args
python mxnet/mxnet_tool.py bill --args
python pytorch/pytorch_tool.py bill --args
</code>

## 命令参数说明
| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key   | 用户的公钥                      | 是      | 无           |
| private\_key  | 用户的私钥                      | 是      | 无           |
| begin\_time   | 起始查询时间（如2017-11-10 7:0:0）  | 是  | 无         |
| end\_time     | 终止查询时间2017-11-10 17:0:0    | 是      | 无            |
| limit		| 返回计费信息条数		  | 否  | 0，返回所有信息 |
| offset 	| 首条计费信息的相对偏移量，limit非0时有效         | 否 | 0，无偏移 |
| project\_id   | 项目组id                      | 否      | 用户默认项目组     |
| region        | 训练任务执行所在地域                 | 否      | 默认地域（华北（北京））   |
| zone          | 训练任务所在可用区                  | 否      | 默认可用区  |

## 返回值说明
**返回值显示全部任务的计费信息**，返回格式如下：
JOB\_NAME: 任务名字; JOB\_ID: 任务ID; EXEC\_TIME: 执行时间（秒）; TOTAL\_PRICE: 总开销（元，精确到0.01元）

## 命令操作案例
<code>
python tf_tool.py bill --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --begin_time='<BEGIN_TIME>'
​    --end_time='<END_TIME>'
</code>

