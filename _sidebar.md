* AI训练服务 UAI-Train
    * [概览](ai/uai-train/overview)
    * 产品简介
        * [什么是AI训练服务](ai/uai-train/introduction/uaitrain)
        * [交互式训练](ai/uai-train/introduction/interactive-job)
        * 分布式训练
            * [分布式训练简介](ai/uai-train/introduction/distructive-job/intro)
            * [TensorFlow分布式训练](ai/uai-train/introduction/distructive-job/tensorflow)
            * [MXNet分布式训练](ai/uai-train/introduction/distructive-job/mxnet)
        * [产品优势](ai/uai-train/introduction/feature)
        * [产品更新记录](ai/uai-train/introduction/updates)
    * [产品定价](ai/uai-train/price)
    * 快速上手
        * [开始使用UAI Train](ai/uai-train/set-up/start)
        * [开通UAI Train服务](ai/uai-train/set-up/active)
        * 快速上手（MNIST案例）
            * [MNIST 介绍](ai/uai-train/set-up/tf-mnist/intro)
            * [环境准备](ai/uai-train/set-up/tf-mnist/prepare)
            * [创建UHub镜像仓库](ai/uai-train/set-up/tf-mnist/uhub)
            * [打包镜像](ai/uai-train/set-up/tf-mnist/pack)
            * [平台训练](ai/uai-train/set-up/tf-mnist/train)
            * [获取结果](ai/uai-train/set-up/tf-mnist/result)
            * [训练代码简介](ai/uai-train/set-up/tf-mnist/coding)
            * [自定义镜像打包](ai/uai-train/set-up/tf-mnist/self-pack)
            * [本地Mnist测试](ai/uai-train/set-up/tf-mnist/local-test)
    * 基础环境
        * [ubuntu云主机申请](ai/uai-train/basic/ubuntu)
        * [Docker使用指南](ai/uai-train/basic/docker)
        * [UHub使用指南](ai/uai-train/basic/uhub)
        * UFile使用指南
            * [创建UFile空间](ai/uai-train/basic/ufile/create)
            * [以网盘方式访问UFile](ai/uai-train/basic/ufile/ufuse)
        * [UFS使用指南](ai/uai-train/basic/ufs)
        * [UDisk使用指南](ai/uai-train/basic/udisk)
        * [账户公私钥获取](ai/uai-train/basic/key)
    * 用户指南
        * 开发综述
            * [设计原理](ai/uai-train/guide/general/principle)
            * [开发综述](ai/uai-train/guide/general/dev-principle)
            * [UCloud环境准备](ai/uai-train/guide/general/ucloud-env)
            * [本地环境准备](ai/uai-train/guide/general/local-env)
        * TensorFlow 开发指南
            * [系统镜像](ai/uai-train/guide/tensorflow/packages)
            * [本地环境](ai/uai-train/guide/tensorflow/local)
            * [API代码](ai/uai-train/guide/tensorflow/coding)
            * [打包镜像](ai/uai-train/guide/tensorflow/packing)
            * [自定义打包镜像](ai/uai-train/guide/tensorflow/userpack)
            * [TF-MNIST开发案例](ai/uai-train/guide/tensorflow/tf-mnist)
            * [TF-Im2txt开发案例](ai/uai-train/guide/tensorflow/tf-im2txt)
        * Caffe 开发指南
            * [系统镜像](ai/uai-train/guide/caffe/packages)
            * [本地环境](ai/uai-train/guide/caffe/local)
            * [API代码](ai/uai-train/guide/caffe/coding)
            * [打包镜像](ai/uai-train/guide/caffe/packing)
            * [自定义打包镜像](ai/uai-train/guide/caffe/userpack)
            * [MNIST开发案例](ai/uai-train/guide/caffe/mnist)
        * Keras 开发指南
            * [系统镜像](ai/uai-train/guide/keras/packages)
            * [本地环境](ai/uai-train/guide/keras/local)
            * [API代码](ai/uai-train/guide/keras/coding)
            * [打包镜像](ai/uai-train/guide/keras/packing)
            * [自定义打包镜像](ai/uai-train/guide/keras/userpack)
            * [MNIST开发案例](ai/uai-train/guide/keras/mnist)
        * MXNet 开发指南
            * [系统镜像](ai/uai-train/guide/mxnet/packages)
            * [本地环境](ai/uai-train/guide/mxnet/local)
            * [API代码](ai/uai-train/guide/mxnet/coding)
            * [打包镜像](ai/uai-train/guide/mxnet/packing)
            * [自定义打包镜像](ai/uai-train/guide/mxnet/userpack)
            * [MNIST开发案例](ai/uai-train/guide/mxnet/mnist)
        * Pytorch 开发指南
            * [本地环境](ai/uai-train/guide/pytorch/local)
            * [API代码](ai/uai-train/guide/pytorch/coding)
            * [打包镜像](ai/uai-train/guide/pytorch/packing)
            * [MNIST开发案例](ai/uai-train/guide/pytorch/mnist)
        * Python命令行操作指南
            * [create-创建训练任务](ai/uai-train/guide/scripts/create)
            * [delete-删除训练任务](ai/uai-train/guide/scripts/delete)
            * [stop-停止训练任务](ai/uai-train/guide/scripts/stop)
            * [info-查询任务信息](ai/uai-train/guide/scripts/info)
            * [list-查询任务详细信息](ai/uai-train/guide/scripts/list)
            * [日志topic（topic）](ai/uai-train/guide/scripts/topic)
            * [实时日志输出（log）](ai/uai-train/guide/scripts/log)
            * [url-查询TensorBoard URL](ai/uai-train/guide/scripts/url)
            * [计费信息（bill）](ai/uai-train/guide/scripts/bill)
            * [获取任务配置（conf）](ai/uai-train/guide/scripts/conf)
            * [pack-自定义镜像打包](ai/uai-train/guide/scripts/self-pack)
        * 控制台界面操作指南
            * [创建任务](ai/uai-train/guide/console/create)
            * [中止任务](ai/uai-train/guide/console/stop)
            * [删除任务](ai/uai-train/guide/console/delete)
            * [访问任务Tensorboard](ai/uai-train/guide/console/tensorboard)
            * [创建交互式训练任务](ai/uai-train/guide/console/createinterjob)
            * [访问Jupyter](ai/uai-train/guide/console/jupyter)
            * [保存交互式训练镜像](ai/uai-train/guide/console/saveinterjobimg)
            * [中止交互式训练任务](ai/uai-train/guide/console/stopinterjob)
            * [启动交互式训练任务](ai/uai-train/guide/console/startinterjob)
            * [删除交互式训练任务](ai/uai-train/guide/console/deleteinterjob)
    * 实践场景
        * CIFAR10图片分类
            * [案例介绍](ai/uai-train/cases/cifar/intro)
            * [建立镜像](ai/uai-train/cases/cifar/img)
            * [模型训练](ai/uai-train/cases/cifar/train)
            * [在线服务](ai/uai-train/cases/cifar/infer)
        * 基于TF-Slim的图片分类
            * [TF-Slim介绍](ai/uai-train/cases/slim/intro)
            * [生成tfrecord文件](ai/uai-train/cases/slim/tfrecord)
            * [模型训练](ai/uai-train/cases/slim/train)
            * [在线服务](ai/uai-train/cases/slim/infer)
        * 物体分类retrain案例
            * [案例介绍](ai/uai-train/cases/retrain/intro)
            * [数据集](ai/uai-train/cases/retrain/data)
            * [模型准备](ai/uai-train/cases/retrain/prep-model)
            * [模型训练](ai/uai-train/cases/retrain/train)
            * [打包镜像](ai/uai-train/cases/retrain/pack)
            * [在线推理服务](ai/uai-train/cases/retrain/infer)
        * 物体识别
            * [案例介绍](ai/uai-train/cases/obj-detect-tf/intro)
            * [数据准备](ai/uai-train/cases/obj-detect-tf/data)
            * [自定义数据](ai/uai-train/cases/obj-detect-tf/data-ud)
            * [数据格式转换](ai/uai-train/cases/obj-detect-tf/data-trans)
            * [模型训练](ai/uai-train/cases/obj-detect-tf/objtrain)
            * [打包镜像](ai/uai-train/cases/obj-detect-tf/obj-packing)
            * [在线服务](ai/uai-train/cases/obj-detect-tf/obj-infer)
        * CRNN-字符识别
            * [案例介绍](ai/uai-train/cases/crnn/intro)
            * [数据准备](ai/uai-train/cases/crnn/data)
            * [自定义数据](ai/uai-train/cases/crnn/data-ud)
            * [数据格式转换](ai/uai-train/cases/crnn/tfrecords)
            * [模型训练](ai/uai-train/cases/crnn/train)
            * [在线服务](ai/uai-train/cases/crnn/infer)
        * CRNN-中文字符识别
            * [案例介绍](ai/uai-train/cases/crnn-chinese/intro)
            * [生成镜像](ai/uai-train/cases/crnn-chinese/imgprep)
            * [数据准备](ai/uai-train/cases/crnn-chinese/data)
            * [生成tfrecords文件](ai/uai-train/cases/crnn-chinese/tfrecords)
            * [模型训练](ai/uai-train/cases/crnn-chinese/train)
            * [在线服务](ai/uai-train/cases/crnn-chinese/infer)
        * 图片转文字标注
            * [案例介绍](ai/uai-train/cases/im2txt/intro)
            * [环境和数据准备](ai/uai-train/cases/im2txt/prepare)
            * [自定义数据](ai/uai-train/cases/im2txt/prep-ud)
            * [文件格式转换](ai/uai-train/cases/im2txt/transform)
            * [模型训练](ai/uai-train/cases/im2txt/train)
            * [打包镜像](ai/uai-train/cases/im2txt/pack)
            * [在线服务](ai/uai-train/cases/im2txt/infer)
    * 免费资源
        * [基础Docker镜像](ai/uai-train/resource/docker)
        * [开源案例](ai/uai-train/resource/example)
        * [学习视频](ai/uai-train/resource/video)
    * [FAQ](ai/uai-train/faq)
        





    
   
   
    
        