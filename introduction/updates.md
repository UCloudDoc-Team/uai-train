{{indexmenu_n>10}}

# 产品更新记录

## UAI Train 交互式版本发布 - 2018.12.05
新特性1：新增Jupyter Notebook交互式训练模式，提供开箱即用的AI开发环境 
新特性2：新增UDisk数据源，支持块存储

## UAI Train 私有化版本 - 2018.09.30
新特性1：新增私有化版本，支持对接私有云账号、计费、数据存储（S3,NFS）
新特性2：基于K8S资源调度，便于私有化部署及运维管理 

## UAI Train 分布式版本发布 - 2018.05.04
新特性1：支持一键Tensorflow、MXNet分布式训练 
新特性2：最多支持8节点（每节点4张P40显卡）共同训练，多节点交互模式下性能衰减不显著，基本线性增长 

**Tensorflow**：ImageNet数据集，input size 224x224，batch_size 256, 数据格式tfrecord 

| Tensorflow | ResNet101(PIC/每单个节点每秒) |
| ---------- | ----------------------------- |
| 8 * 4P40    | 200                   |
| 4 * 4P40    | 246                   |
| 2 * 4P40    | 295                   |
| 1 * 4P40    | 307                   |

**MXNet**：ImageNet数据集，input size 299x299，batch_size 256, 数据格式rec 

| MXNet | ResNet101(PIC/每单个节点每秒) |
| ----- | ----------------------------- |
| 8 * 4P40    | 214                   |
| 4 * 4P40    | 215                   |
| 2 * 4P40    | 215                   |
| 1 * 4P40    | 223                   |

## UAI Train 开放Docker镜像和Case Study - 2018.01.31
UAI Train公开所支持的AI Docker镜像：[[ai:uai-train:basic:docker]] 
UAI Train公开部分开源AI 算法Case Study：https://github.com/ucloud/uai-sdk/tree/master/examples 

## UAI Train版本更新 - 2017.11.28
新特性1：Python版SDK发布，支持自动化脚本接入
新特性2：新增K80显卡，选择更多，更经济 
新特性3：数据加载速度提升1倍以上，从容面对海量数据 
当前支持AI框架：Tensorflow、MXNet、Keras、Caffe、Pytorch 

## UAI Train正式发布 - 2017.09.01
**特性1：性能强劲** 
支持最高1机4卡P40节点，单机高达48TFlops的单精度计算能力。通过分布式扩展，最高可达192TFlops 单精度计算能力。
**特性2：训练任务一站式托管** 
系统自动进行GPU节点调度，数据上传下载，任务容灾等工作，无须用户担心。
**特性3：训练任务状态追踪** 
提供训练任务标准输出日志转发和TensorBoard转发功能，用户可实时监控训练状态。
**特性4：按需付费** 
按照实际计算消耗付费，收费更灵活、便捷，无需担心资源浪费。

