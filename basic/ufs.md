

# UFS使用指南
UFS 是面向UCloud云主机提供简单、可扩展、高可靠、高性能的标准POSIX文件共享访问方式。UFS 同时可以作为UAI Train训练节点的输入、输出数据源。您可以为UAI Train训练任务指定UFS的文件路径作为数据输入源，也可以指定UFS的文件路径作为数据的输出源。

## 申请UFS存储
在使用UFS存储作为AI Train平台的数据源之前，您需要在[UFS的产品界面](https://console.ucloud.cn/ufs/ufs)申请UFS存储，详细操作方法请参见[UFS](ufs/README)。

## UFS 路径格式说明
UAI Train中使用的UFS路径包括两部分：

1. UFS 挂载点IP，格式为 ip:/xxx
2. 所使用的数据所在的UFS文件系统中的路径，格式为/aaa/bbb/
   所以最终的填写入UFS路径的地址为：ip:/xxx/aaa/bbb/

## UFS 的使用限制

  * UFS的单个文件不能大于8GB
  * UFS针对大量小文件的文件夹访问性能比较弱（比如含有大量图片文件的文件夹），因此请尽量避免在AI训练数据为图片的情况下使用UFS作为数据输入的存储。请将图片文件转化为AI框架可以使用的数据文件，例如tensorflow使用的tfrecord，MXNet的Rec

