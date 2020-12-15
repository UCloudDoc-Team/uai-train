

#  API代码
## 获取方法
[地址](https://github.com/ucloud/uai-sdk)
<code>
git clone https://github.com/ucloud/uai-sdk.git
</code>

## PyTorch 相关文件路径
<code>
uai-sdk/

  examples/
     pytorch/
        train/
  uaitrain/
     arch/
        pytorch/
          uargs.py
  uaitrain_tool/
     pytorch/
        pytorch_tool.py
</code>

## 简介

### uaitrain/arch/pytorch/uargs.py
uaitrain/arch/pytorch/uargs.py 定义了UAI Train在运行PyTorch训练任务时所需的参数定义，此类参数均为固定参数。

#### 固定参数
| 参数 | 默认值 | 说明 |
| ---- | ------ | ---- |
| \-\-work\_dir    | /data         | 默认的执行路径，UAI Train打包工具会默认将用户指定的训练代码放入该路径下，详细可见[](uai-train/guide/mxnet/mnist) |
| \-\-data\_dir    | /data/data    | 默认input数据存放路径，UAI Train会将US3中的input数据下载到该路径下                               |
| \-\-output\_dir  | /data/output  | 默认输出路径，checkpoint和模型文件需要输出到该路径下，UAI Train会在训练完成后将该目录上传                          |
| \-\-log\_dir     | /data/output  |                                                                                 |
| \-\-num\_gpus    | <#num\_gpus>  | GPU数量，UAI Train会根据训练节点实际的GPU数量生成该参数，GPU编号为递增0，1，2，3...                          |

固定参数为系统自动生成，用户不需要做特殊设置。

### uaitrain_tool/pytorch/pytorch_tool.py
pytorch\_tool.py 工具支持镜像打包功能，详细使用方法可参见[](uai-train/guide/pytorch/packing)

