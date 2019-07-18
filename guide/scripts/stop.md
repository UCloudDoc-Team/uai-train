{{indexmenu_n>3}}


===== 停止训练任务（Stop）=====
使用该命令可以停止正在执行的训练任务，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py stop --args
python tf/tf_tool.py stop --args
python caffe/caffe_tool.py stop --args
python keras/keras_tool.py stop --args
python mxnet/mxnet_tool.py stop --args
python pytorch/pytorch_tool.py stop --args
</code>

====命令参数说明====
^ 参数                  ^                          说明                  ^      是否必需     ^    默认值     ^
| public\_key         | 用户的公钥                                              | 是              |        无     |
| private\_key        | 用户的私钥                                              | 是              |        无     |
| job\_id                | 任务ID（Create 命令的返回值）                 | 是              |      无      |
| project\_id         | 项目组id                                                  | 否               |        用户默认项目组   |
| region               | 训练任务执行所在地域                                 | 否               |        默认地域（北京）   |
| zone                 | 训练任务所在可用区                                    | 否              |        默认可用区（北京二）   |


====返回值说明====
**返回值显示停止是否成功**

====命令操作案例====
<code>
python tf_tool.py stop --public_key='<PUB_KEY>' \
    --private_key='<PRI_KEY>' \
    --job_id=’<JOB_ID>’
</code>