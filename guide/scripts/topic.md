{{indexmenu_n>6}}

# topic-查询日志topic列表
使用该命令可以查询训练任务的详细信息，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py topic --args

python tf/tf_tool.py topic --args
python caffe/caffe_tool.py topic --args
python keras/keras_tool.py topic --args
python mxnet/mxnet_tool.py topic --args
python pytorch/pytorch_tool.py topic --args
</code>

## 命令参数说明

| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key   | 用户的公钥                | 是      |  无           |
| private\_key  | 用户的私钥                | 是      |  无           |
| job\_id       | 任务ID（Create 命令的返回值）  | 是      |  无           |
| project\_id   | 项目组id                | 否      |  用户默认项目组     |
| region        | 训练任务执行所在地域           | 否      |  默认地域（北京二）    |
| zone          | 训练任务所在可用区            | 否      |  默认可用区  |

## 返回值说明
**返回训练日志的topic列表**

## 命令操作案例
查看某任务的实时日志
<code>
python tf_tool.py topic --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --job_id=’<JOB_ID>’
</code>

