{{indexmenu_n>5}}

# 平台训练
现在我们可以使用UAI Train训练平台的GPU来训练Mnist模型了。

  * 首先上传训练数据
  * 发起训练任务

## 向UFile 上传训练数据
向UFile上传数据需要做如下操作：

  * 创建UFile Bucket
  * 下载UFile 操作工具
  * 上传数据

其他UFIle操作请查看[[ai:uai-train:basic:ufile]]

### 创建UFile Bucket
我们访问[[https://console.ucloud.cn/ufile/ufile]]，点击创建存储空间：
{{:ai:uai-train:tutorial:tf-mnist:ufile-create.png?600|}}

然后选择北京地域创建一个名为uai-demo的存储空间（你可以创建自己命名的存储空间）: 
{{:ai:uai-train:tutorial:tf-mnist:ufile-create2.png?600|}}

### 下载UFile操作工具
我们直接下载Linux的操作工具，其他工具可以在[[ai:uai-train:basic:ufile]]查看

  *  [[http://tools.ufile.ucloud.com.cn/filemgr-linux64.tar.gz|filemgr-linux64]]

<code>
$ cd ~

$ wget http://tools.ufile.ucloud.com.cn/filemgr-linux64.tar.gz
$ tar -zxf filemgr-linux64.tar.gz
$ cd filemgr-linux64
</code>
之后我们就可以操作UFile了

### 使用UFile工具上传训练数据
首先需要修改config.cfg设置公私钥：
<code>
$ vim config.cfg
</code>
将**public\_key**和**private\_key**修改成你自己账号的公私钥，然后将**proxy\_host**改为www.ufile.cn-north-04.ucloud.cn，因为我们的云主机在北京D机房，其他机房的配置可以参考[[ai:uai-train:basic:ufile:file]]的说明。

之后我们就可以用如下命令上传数据
<code>
./filemgr-linux64 --action mput --bucket uai-demo --dir /data/mnist/data/  --trimpath /data/ --threads 4
</code>
这里我们使用mput来上传数据：
  * ufile的目标bucket为 uai-demo
  * 需要上传的数据为 /data/mnist/data/目录下的数据
  * 我们利用trimpath 将上传数据的路径截断，即上传后数据的路径为uai-demo.cn-bj.ufileos.com/mnist/data/

## 发起训练任务
我们可以在https://console.ucloud.cn/uaitrain/manage界面创建训练任务：
{{:ai:uai-train:tutorial:tf-mnist:train-step1.png?600|}}

我们首先选择训练节点（1* P40），然后填写好公私钥（用于数据访问授权），具体请您参考[[ai:uai-train:basic:key|]]。
{{:ai:uai-train:tutorial:tf-mnist:train-step2.png?600|}}

之后选择训练相关的参数：
  * 训练镜像（界面会自动关联你的uhub镜像库）
  * 输入数据源（UFile）
  * 输出数据源（UFile）
  * 训练的执行命令：/data/mnist\_summary.py \-\-max\_step=2000，我们需要给出入口代码和相关训练参数
{{:ai:uai-train:tutorial:tf-mnist:train-step3.png?600|}}

### 如何获取输入数据源的地址
我们可以在UFile的界面获取输入数据的ufile地址，操作如下：

1.进入[[https://console.ucloud.cn/ufile/ufile]]，点击你的ufile bucket（本例子为uai-demo）
2.点击获取地址 
{{:ai:uai-train:tutorial:tf-mnist:train-ufile-step1.png?600|}}

3.截取地址的一部分前缀 
{{:ai:uai-train:tutorial:tf-mnist:train-ufile-step2.png?600|}}

### 发起训练任务
我们点击确认按钮就可以发起训练。其他训练操作相关请参考[[ai:uai-train:set-up:how-to-use]]

