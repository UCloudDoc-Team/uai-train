{{indexmenu_n>7}}


===== 实时日志输出（log）=====
使用该命令可以查询训练任务的详细信息，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py log --args
python tf/tf_tool.py log --args
python caffe/caffe_tool.py log --args
python keras/keras_tool.py log --args
python mxnet/mxnet_tool.py log --args
python pytorch/pytorch_tool.py log --args
</code>

====命令参数说明====
^ 参数              ^  说明                    ^  是否必需  ^  默认值         ^
| public\_key     | 用户的公钥                  | 是      |  无           |
| private\_key    | 用户的私钥                  | 是      |  无           |
| job\_id         | 任务ID（Create 命令的返回值）    | 是      |  无           |
| log\_topic\_id  | 日志topic id，由topic命令获取  | 是      |  无           |
| project\_id     | 项目组id                  | 否      |  用户默认项目组     |
| region          | 训练任务执行所在地域             | 否      |  默认地域（北京）    |
| zone            | 训练任务所在可用区              | 否      |  默认可用区（北京二）  |


====返回值说明====
**会实时输出该任务的日志信息，如果任务已经完成，则输出最后50行日志**

====命令操作案例====
查看某任务的实时日志
<code>
python tf_tool.py log --public_key='<PUB_KEY>' \
    --private_key='<PRI_KEY>' \
    --job_id=’<JOB_ID>’
    --log_topic_id=’<LOG_TOPIC_ID>’
</code>