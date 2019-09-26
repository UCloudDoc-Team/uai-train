{{indexmenu_n>5}}

# 模型训练
我们使用[[ai:uai-train:case:crnn-chinese:imagepre|]]中生成的镜像进行模型训练。

## 训练
我们可以先在本地进行训练，测试训练能否正常进行，再在UAI-Train平台上进行训练。

### 本地训练

我们需要把训练需要的数据文件映射到uhub.service.ucloud.cn/uai\_demo/crnn\_cpu:latest。
<code>
sudo docker run -it -v /data/data:/data/data -v /data/output:/data/output  uhub.service.ucloud.cn/uai_demo/crnn_cpu:latest  /bin/bash -c "/usr/bin/python /data/code/tools/train_shadownet.py"
</code>
我们可以在/data/output中查看相应的训练输出。

### 平台训练
**上传训练数据**

我们需要将/data/data下的文件上传到UFile或者UFS中，这里我们以UFile平台为例。
我们使用事先下载的UFile工具，进入filemgr-linux64.elf文件夹，通过UFile平台的数据上传命令将/data/data下的数据上传至UFile中，并以/crnn\_poem/data/作为前缀，你可以自由地修改上传数据的前缀；
<code>
./filemgr-linux64 --action mput --bucket uai-demo --dir /data/data/tfrecords/ --prefix /crnn_poem/data/tfrecords/

./filemgr-linux64 --action mput --bucket uai-demo --dir /data/data/char_dict/ --prefix /crnn_poem/data/char_dict/
</code>

**填写数据输入路径**

在UAI-Train平台上训练的具体操作步骤可以参考[[ai:uai-train:tutorial:tf-mnist:train|]]

如果你的数据存放在UFile平台的/crnn\_poem/data/下面，你的数据路径可以填写为
<code>
http://uai-demo.ufile.ucloud.com.cn//crnn_poem/data/
</code>

**执行训练命令**


训练的执行命令为：
<code>
/data/code/tools/train_shadownet.py 
</code>
我们可以在设置的输出路径中查看模型训练结果。


