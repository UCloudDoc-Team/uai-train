

# UDisk使用指南
UFS 是面向UCloud云主机提供持久化存储空间的块设备硬盘。其具有独立的生命周期，基于网络分布式访问，为云主机提供的数据大容量、高可靠、可扩展、高易用、低成本的硬盘。您可以为UAI Train训练任务指定UDisk的文件路径作为数据输入源，也可以指定UDisk的文件路径作为数据的输出源。

## 申请UDisk存储
在使用UDisk存储作为AI Train平台的数据源之前，您需要在UDisk的产品界面(https://console.ucloud.cn/uhost/udisk)申请UDisk存储,需要经过以下几个步骤：

1. 创建云硬盘[[storage_cdn:udisk:userguide:create]] 

2. 挂载云硬盘[[storage_cdn:udisk:userguide:mount]] 

3. 格式化云硬盘[[storage_cdn:udisk:userguide:format]] 

4. 卸载云硬盘[[storage_cdn:udisk:userguide:umount]] 

说明：UAI Train使用的云硬盘必须是**格式化过，并且没有挂载在任何云主机上**的

## UDisk 路径格式说明
UAI Train中使用的UDisk路径包括两部分：

1. UDisk云盘资源ID，格式为 bs-xxxx；
2. 所使用的数据所在的UDisk文件系统中的路径，格式为/aaa/bbb/
      所以最终的填写入UDisk路径的地址为：bs-xxxx/xxx/aaa/bbb/

## UDisk 的使用限制

  * UDisk只支持单点挂载，因此同一个UDisk同时只能支持一个UAI Train训练任务执行

