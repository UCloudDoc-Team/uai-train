{{indexmenu_n>3}}

===== API代码 =====
**UAI Train的Keras系统的backend为TensorFlow，因此Keras的API会部分依赖TensorFlow的API**

===== 获取方法 =====
[[https://github.com/ucloud/uai-sdk]]
<code>
git clone https://github.com/ucloud/uai-sdk.git
</code>

===== Keras相关文件路径 =====
<code>
uai-sdk/
  examples/
    keras/
       train/
  uaitrain/
    arch/
      tensorflow/
         uflag.py
  uaitrain_tool/
    keras/
      keras_tool.py
</code>

===== 简介 =====

=== uaitrain/arch/tensorflow/uflag.py （Keras, TensorFlow公用）===
uaitrain/arch/tensorflow/uflag.py 定义了UAI Train在运行Keras训练任务时所需的参数定义，其中包括固定参数和可变参数。\\
由于Keras使用的是tensorflow的backend，因此可以直接引用TensorFlow的参数

== 固定参数 ==
^  参数  ^ 默认值  ^ 说明  ^
|\-\-work\_dir    | /data | 默认的执行路径，UAI Train打包工具会默认将用户指定的训练代码放入该路径下，详细可见[[ai:uai-train:guide:tensorflow:mnist]] |
|\-\-data\_dir    | /data/data  | 默认input数据存放路径，UAI Train会将UFile中的input数据下载到该路径下 |
|\-\-output\_dir   | /data/output | 默认输出路径，checkpoint和模型文件需要输出到该路径下，UAI Train会在训练完成后将该目录上传 |
|\-\-log\_dir   | /data/output | 默认的tensorboard文件存放路径，UAI Train会在训练完成后将该目录上传 |
|\-\-num\_gpus   | <#num\_gpus> | GPU数量，UAI Train会根据训练节点实际的GPU数量生成该参数 |

== 可变参数 ==
^  参数  ^ 默认值  ^ 说明  ^
|\-\-max_step | 0 | 训练最大Step数，UAI Train系统可以识别该参数，后续将提供训练进度功能（目前不支持）|

=== uaitrain_tool/keras/keras_tool.py ===
keras\_tool.py 工具支持镜像打包功能，详细使用方法可参见[[ai:uai-train:guide:keras:packing]]