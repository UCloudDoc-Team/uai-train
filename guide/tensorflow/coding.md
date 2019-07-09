{{indexmenu_n>3}}

## API代码

## 获取方法

<https://github.com/ucloud/uai-sdk>

    git clone https://github.com/ucloud/uai-sdk.git

## TensorFlow相关文件路径

    uai-sdk/
      examples/
        tensorflow/
           train/
      uaitrain/
        arch/
          tensorflow/
             uflag.py
      uaitrain_tool/
        tf/
          tf_tool.py

## 简介

#### uaitrain/arch/tensorflow/uflag.py

uaitrain/arch/tensorflow/uflag.py 定义了UAI
Train在运行TensorFlow训练任务时所需的参数定义，其中包括固定参数和可变参数。  
\== 固定参数 ==

| 参数                  | 默认值               | 说明                                                                                                                                                      |
| ------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \\-\\-work\\\_dir   | /data             | 默认的执行路径，UAI Train打包工具会默认将用户指定的训练代码放入该路径下，详细可见[tf-mnist](/ai/uai-train/guide/tensorflow/tf-mnist), [tf-im2txt](/ai/uai-train/guide/tensorflow/tf-im2txt) |
| \\-\\-data\\\_dir   | /data/data        | 默认input数据存放路径，UAI Train会将UFile中的input数据下载到该路径下                                                                                                          |
| \\-\\-output\\\_dir | /data/output      | 默认输出路径，checkpoint和模型文件需要输出到该路径下，UAI Train会在训练完成后将该目录上传                                                                                                  |
| \\-\\-log\\\_dir    | /data/output      | 默认的tensorboard文件存放路径，UAI Train会在训练完成后将该目录上传                                                                                                             |
| \\-\\-num\\\_gpus   | \<\#num\\\_gpus\> | GPU数量，UAI Train会根据训练节点实际的GPU数量生成该参数                                                                                                                     |

##### 可变参数

| 参数              | 默认值 | 说明                                              |
| --------------- | --- | ----------------------------------------------- |
| \\-\\-max\_step | 0   | 训练最大Step数，UAI Train系统可以识别该参数，后续将提供训练进度功能（目前不支持） |

#### uaitrain\_tool/tf/tf\_tool.py

tf\\\_tool.py
工具支持镜像打包功能，详细使用方法可参见[packing](/ai/uai-train/guide/tensorflow/packing)
