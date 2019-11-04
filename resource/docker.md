

# 基础Docker镜像
所有镜像都可以通过UCloud内网uhub.service.ucloud.cn拉取，如果从公网拉取，请通过uhub.ucloud.cn.

## TensorFlow
<code>
**TensorFlow2.0.0a py3 (cuda10)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.5\_tensorflow-2.0.0a:v1.0 

**TensorFlow2.0.0a py2 (cuda10)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_tensorflow-2.0.0a:v1.0 
**TensorFlow1.13.1 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-16.04_python-2.7_tensorflow-1.13.1:v1.0 
**TensorFlow1.13.1 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu_uaitrain_ubuntu-16.04_python-3.5_tensorflow-1.13.1:v1.0 
**TensorFlow1.12.0 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.5\_tensorflow-1.12.0:v1.0 
**TensorFlow1.12.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_tensorflow-1.12.0:v1.0 
**TensorFlow1.9.0 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_tensorflow-1.9.0:v1.0 
**TensorFlow1.9.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_tensorflow-1.9.0:v1.0 
**TensorFlow1.8.0 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_tensorflow-1.8.0:v1.0 
**TensorFlow1.8.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_tensorflow-1.8.0:v1.0 
**TensorFlow1.7.0 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_tensorflow-1.7.0:v1.0 
**TensorFlow1.7.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_tensorflow-1.7.0:v1.0 
**TensorFlow1.6.0 py3 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_tensorflow-1.6.0:v1.0 
**TensorFlow1.6.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_tensorflow-1.6.0:v1.0 
**TensorFlow1.5.0 py2 (cuda9)**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_tensorflow-1.5.0:v1.0 
**TensorFlow1.4.0 py2**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_tensorflow-1.4.0:v1.0 
**TensorFlow1.4.0 py3**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.4.0:v1.0 
**TensorFlow1.3.0 py3**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6.2\_tensorflow-1.3.0:v1.0 
**TensorFlow1.2.0 py2**:uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_tensorflow-1.2.0:v1.0 
</code>

### Case Study(TensorFlow)
<code>
**object-detection(1.5)**: uhub.service.ucloud.cn/uaishare/tf-obj-detect:tf-1.5 

**object-detection(1.4)**: uhub.service.ucloud.cn/uaishare/tf-obj-detect:tf-1.4 
**deep-speech**: uhub.service.ucloud.cn/uaishare/deepspeech:tf-1.4 
**tensorflow-model-1.8.0(1.5)**:  uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_tensorflow-1.5\_models:v1.8.0 
**tensorflow-model-1.8.0(1.6)**:  uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_tensorflow-1.6\_models:v1.8.0 
</code>

## Caffe
<code>
**Caffe 1.0.0**: uhub.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_caffe-1.0.0:v1.0 
</code>

### Case Study(Caffe)
<code>
**Faster-RCNN**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_caffe-py-faster-rcnn:v1.0 

**RFCN**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_caffe-py-rfcn:v1.0 
</code>

### Case Study(Caffe2)
<code>
**Detectron**: uhub.service.ucloud.cn/uai\_dockers/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_caffe2:v1.0
</code>

## MXNet
<code>
**MXNet 1.0.0 (cuda9)**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7.6\_mxnet-1.0.0cuda9:v1.0 

**MXNet 1.0.0 (cuda8)**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_mxnet-1.0.0:v1.0 
**MXNet 0.11.0**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_mxnet-0.11.0:v1.0 
</code>

## Keras
<code>
**Keras 2.1.6 py3(tf-1.8)**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_keras-2.1.6:v1.0 

**Keras 2.1.6 py2(tf-1.8)**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-2.7\_keras-2.1.6:v1.0 
**Keras 2.0.8**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_keras-2.0.8:v1.0 
</code>

## Pytorch
<code>
**Pytorch-0.2.0**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-14.04\_python-2.7.6\_pytorch-0.2.0:v1.0 

**Pytorch-0.4**: uhub.service.ucloud.cn/uaishare/gpu\_uaitrain\_ubuntu-16.04\_python-3.6\_pytorch-0.4:v1.0 
</code>
