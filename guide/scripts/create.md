{{indexmenu_n>1}}


===== 创建训练任务（Create）=====
使用该命令可以创建新的训练任务，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py create --args
python tf/tf_tool.py create --args
python caffe/caffe_tool.py create --args
python keras/keras_tool.py create --args
python mxnet/mxnet_tool.py create --args
python pytorch/pytorch_tool.py create --args
</code>

====命令参数说明====
^ 参数                   ^  说明                                         ^  是否必需  ^  默认值         ^
| public\_key          | 用户的公钥                                       | 是      |  无           |
| private\_key         | 用户的私钥                                       | 是      |  无           |
| job\_name            | 任务名字                                        | 是      |  无           |
| node\_type           | 节点类型，包括：1-P40, 2-P40, 4-P40                 | 是      |  无           |
| code\_uhub\_path     | Docker镜像路径，可以从uhub产品处获取                     | 是      |  无           |
| docker\_cmd          | Python执行命令，注：请用 ”” 将命令包括起来                  | 是      |  无           |
| max\_exec\_time      | 最大执行时间，小时为单位，必须>6                           | 是      |  无           |
| data\_ufile\_path    | 输入数据在UFile/UFS的路径                           | 是      |  无           |
| output\_ufile\_path  | 输出数据在UFile/UFS的路径                           | 是      |  无           |
| node\_num            | 用于分布式训练。分布式训练任务所需的节点数量                      | 否      |  无           |
| dist\_ai\_frame      | 用于分布式训练。分布式训练任务使用的AI框架（支持tensorflow、mxnet）  | 否      |  无           |
| project\_id          | 项目组id                                       | 否      |  用户默认项目组     |
| region               | 训练任务执行所在地域                                  | 否      |  默认地域（北京）    |
| zone                 | 训练任务所在可用区                                   | 否      |  默认可用区（北京二）  |
| job\_memo            | 任务说明                                        | 否      |  空           |
| business\_group      | 业务组（如果填写，该业务组必须存在）                          | 否      |  空           |

**注：node\_type具体说明**
^ node\_type ^ 说明 ^ 
| 1-P40 | 1机1卡P40 |
| 2-P40 | 1机2卡P40 |
| 4-P40 | 1机4卡P40 |

====返回值说明====
**返回值为 JOB\_ID，该ID为训练任务的唯一标示**

====命令操作案例====
单节点训练 \\
<code>
python tf_tool.py create --public_key='<PUB_KEY>' \
    --private_key='<PRI_KEY>' \
    --job_name='test'  \
    --job_memo='test' \
    --node_type='1-P40' \
    --code_uhub_path='<UHUB_IMG_PATH>'  \
    --docker_cmd="/data/mnist_summary.py --learning_rate=0.002 --max_step=10" \
    --max_exec_time=6  \
    --data_ufile_path='<UFILE_DATA_PATH>'  \
    --output_ufile_path='<UFILE_OUTPUT_PATH>'
</code>

分布式训练 \\
<code>
python tf_tool.py create --public_key='<PUB_KEY>' \
    --private_key='<PRI_KEY>' \
    --job_name='test'  \
    --job_memo='test' \
    --node_type='4-P40' \
    --code_uhub_path='<UHUB_IMG_PATH>'  \
    --docker_cmd="/data/mnist_summary.py --learning_rate=0.002 --max_step=10" \
    --max_exec_time=6  \
    --data_ufile_path='<UFILE_DATA_PATH>'  \
    --output_ufile_path='<UFILE_OUTPUT_PATH>'
    --node_num=4
    --dist_ai_frame=tensorflow
</code>