{{indexmenu_n>3}}

===== API代码 =====
===== 获取方法 =====
[[https://github.com/ucloud/uai-sdk]]
<code>
git clone https://github.com/ucloud/uai-sdk.git
</code>

===== Caffe相关文件路径 =====
<code>
uai-sdk/
  examples/
    caffe/
       train/
  uaitrain/
    arch/
      caffe/
         train.py
  uaitrain_tool/
    caffe/
      caffe_tool.py
</code>

===== 简介 =====

=== uaitrain/arch/caffe/train.py ===
uaitrain/arch/caffe/train.py 是Caffe训练的特定入口文件，由于AI Training平台统一采用python的接口启动训练任务，因此我们需要该train.py 文件作为入口 \\

train.py 有如下参数：
== 可变参数 ==
^  参数  ^ 默认值  ^ 说明  ^ 是否必填 ^
|\-\-solver | "" | Caffe 的 Solver proto 定义文件的路径 | 是 |
|\-\-snapshot | "" | Caffe 训练时用于restore的snapshot文件的路径 | 否 |
|\-\-timing | "" | 是否需要添加训练时间信息 | 否 |

**可变参数中\-\-solver是必填项，指定了Solver proto的路径**, 通常有两种填写方法：
  - /data/xxx，此时solver proto文件需要和train.py 一同打包进/data/ 目录下，详细说明请参见[[ai:uai-train:guide:caffe:packing]]
  - /data/data/xxx，此时solver proto文件需要和数据一同上传至UFIle存储中，训练过程文件将下载至/data/data/目录下。**用户可以通过该方案，在不重上传Docker镜像的基础上，动态修改solver proto 文件**

== 固定参数 ==
^  参数  ^ 默认值  ^ 说明  ^
|\-\-work\_dir    | /data | 默认的执行路径，UAI Train打包工具会默认将用户指定的训练代码放入该路径下，详细可见[[ai:uai-train:guide:caffe:mnist]] |
|\-\-data\_dir    | /data/data  | 默认input数据存放路径，UAI Train会将UFile中的input数据下载到该路径下 |
|\-\-output\_dir   | /data/output | 默认输出路径，checkpoint和模型文件需要输出到该路径下，UAI Train会在训练完成后将该目录上传 |
|\-\-num\_gpus   | <#num\_gpus> | GPU数量，UAI Train会根据训练节点实际的GPU数量生成该参数 |

固定参数为系统自动生成，用户不需要做特殊设置。由于Caffe 会自动进行多GPU训练，因此用户无需关心GPU的配置，train.py已经帮用户自动去使用多GPU训练（如果num\_gpus >1)，当然用户可以自行修改train.py

=== uaitrain_tool/caffe/caffe_tool.py ===
caffe\_tool.py 工具支持镜像打包功能，详细使用方法可参见[[ai:uai-train:guide:caffe:packing]]