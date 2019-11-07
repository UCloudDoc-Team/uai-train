

# MNIST开发案例
本案例所使用的模型和代码基于MXNet 的MNIST案例，您可以在https://github.com/apache/incubator-mxnet/tree/master/example/image-classification 找到MXNet train\_mnist.py的原始的案例。您可以在您可以在https://github.com/ucloud/uai-sdk/tree/master/examples/mxnet/train/mnist/下面下载完整的代码和测试数据集。

## 准备工作
请根据 开发指南>MXNet开发指南>MXnet 本地安装部署开发环境[[ai:uai-train:guide:mxnet:local]]完成所有安装步骤，即完成了基本环境的部署 
从https://github.com/ucloud/uai-sdk/tree/master/examples/mxnet/train/mnist下载完整的案例，包括：

  * code/ 训练代码，包括train\_mnist.py common/ symbols/，我们对mxnet的原始案例进行了相关的移植和修改，common目录下包含了fit.py 和 find\_mxnet.py，symbols下面包含了mlp.py
  * data/ 测试数据集

## 编写MNIST案例
本案例使用的MNIST模型的训练代码来自MXNet的MNIST案例的train\_mnist.py和common/fit.py, 并做如下修改 

### train_mnist.py
#### 新增import uai-train的uargs.py包
UAI Train 平台对平台自动生成的参数进行了封装，所有定义均保存在 uaitrain.arch.mxnet.uargs.py 中，因此我们直接引用MXNet的uargs.py包 
**添加代码引入uargs.py** \(L30\)：
<code>
#import UAI argument defines
from uaitrain.arch.mxnet import uargs
</code>

**arg parser 增加uai相关参数 ** \(L77\):
<code>
    # Add uai specific arguments
    uargs.add_uai_args(parser)
</code>
这样train\_mnist.py 运行时就会去自动接卸 mxnet.uargs里面定义的参数。

#### 使用 data_dir 作为输入路径
**通过使用 args.data\_dir 指定输入路径**
我们修改了train\_mnist.py 的输入数据路径，由从网络下载改为使用本地数据，修改如下\(L33\):
<code>
def read_data(label, image, data_dir):

​    """
​    download and read data into numpy
​    """
​    with gzip.open(os.path.join(data_dir,label)) as flbl:
​        magic, num = struct.unpack(">II", flbl.read(8))
​        label = np.fromstring(flbl.read(), dtype=np.int8)
​    with gzip.open(os.path.join(data_dir,image), 'rb') as fimg:
​        magic, num, rows, cols = struct.unpack(">IIII", fimg.read(16))
​        image = np.fromstring(fimg.read(), dtype=np.uint8).reshape(len(label), rows, cols)
​    return (label, image)
</code>
本地路径通过os.path.join(data\_dir, label)来拼接，**其中data\_dir则从外部传入**，实际为UAI Train特定的输入参数：**args.data\_dir**，其修改如下\(L52\):
<code>
def get_mnist_iter(args, kv):

​    """
​    create data iterator with NDArrayIter
​    """
​    (train_lbl, train_img) = read_data(
​            'train-labels-idx1-ubyte.gz', 'train-images-idx3-ubyte.gz', args.data_dir)
​    (val_lbl, val_img) = read_data(
​            't10k-labels-idx1-ubyte.gz', 't10k-images-idx3-ubyte.gz', args.data_dir)
</code>

### common/fit.py
#### 使用 output_dir 作为输出路径
**通过使用 args.output\_dir 指定输出路径**
在fit.py 中定义了\_save\_model 和 \_load\_model函数用于保存和加载训练过程中的模型文件，我们在原代码基础上，指定了模型输出、输入路径必须以 args.output\_dir为前缀。
模型输出的修改如下\(L55\):
<code>
def _save_model(args, rank=0):
    if args.model_prefix is None:
        return None

    #Add UAI output_dir path
    model_prefix = os.path.join(args.output_dir, args.model_prefix)
    dst_dir = os.path.dirname(model_prefix)
    if not os.path.isdir(dst_dir):
        os.mkdir(dst_dir)
    return mx.callback.do_checkpoint(model_prefix if rank == 0 else "%s-%d" % (
        model_prefix, rank))
</code>

模型输入的修改如下\(L41\):
<code>
def _load_model(args, rank=0):
    if 'load_epoch' not in args or args.load_epoch is None:
        return (None, None, None)
    assert args.model_prefix is not None

    #Add UAI output_dir path
    model_prefix = os.path.join(args.output_dir, args.model_prefix)
    if rank > 0 and os.path.exists("%s-%d-symbol.json" % (model_prefix, rank)):
        model_prefix += "-%d" % (rank)
    sym, arg_params, aux_params = mx.model.load_checkpoint(
        model_prefix, args.load_epoch)
</code>

## 使用Docker镜像进行测试
因为UAI Train 平台使用Docker运行训练，因此可以使用Docker对训练代码进行测试。
UAI Train平台提供开源的Docker 打包工具，打包方法见[[ai:uai-train:guide:mxnet:packing]] 
打包完成后打包工具会自动生成GPU的镜像和CPU的镜像，命名如下：

  * **CPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>-cpu:<usr-def-tag>
  * **GPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>:<usr-def-tag>

### 使用CPU Docker 测试
打包工具会自动生成CPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for CPU local test: <docker run cmd> ）
直接执行命令即可

### 使用GPU Docker 测试
打包工具会自动生成GPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for GPU local test: <docker run cmd> ）
直接执行命令即可

