

# MNIST开发案例
本案例使用的MNIST模型的训练代码来自pytorch的MNIST案例的mnist.py。（https://github.com/pytorch/examples/tree/master/mnist）
改写后的文件存放在~/uai-sdk/examples/pytorch/train/mnist目录下。该目录包含：

  * code/ 训练代码，包含mnist.py文件
  * data/ 测试数据集

## 准备工作
请根据 本地安装部署开发环境[](uai-train/guide/pytorch/local)完成所有安装步骤，即完成了基本环境的部署 

## 编写MNIST案例
本案例使用的MNIST模型的训练代码来自MXNet的MNIST案例的**mnist.py**, 并做如下修改 

### 1.导入uargs.py模块

首先导入uaitrain.arch.pytorch包的uargs模块，该模块定义了代码中需要使用的命令行参数，然后将参数添加到argparse中。
添加代码引入uargs.py：(L11)

<code>
\# import UAI argument defines
from uaitrain.arch.pytorch import uargs''
</code>
arg parser 增加uai相关参数:(L32)

<code>
\# Add uai specific arguments
uargs.add_uai_args(parser)
</code>
这样mnist.py运行时就能够使用uargs.py里面定义的参数。

### 2.使用 data_dir 作为输入路径
通过使用 args.data\_dir 指定数据的输入路径，由于本例的数据已经提供，故可将参数download设值为False，使其从本地加载。修改如下: 
(L45)
<code>
train_loader = torch.utils.data.DataLoader(
    datasets.MNIST(args.data_dir, train=True, download=False,
                   transform=transforms.Compose([
                       transforms.ToTensor(),
                       transforms.Normalize((0.1307,), (0.3081,))
                   ])),
</code>
(L52)
<code>
test_loader = torch.utils.data.DataLoader(
    datasets.MNIST(args.data_dir, train=False, transform=transforms.Compose([
                       transforms.ToTensor(),
                       transforms.Normalize((0.1307,), (0.3081,))
                   ])),
</code>

### 3.使用 output_dir 作为输出路径
通过使用 args.output\_dir 指定输出路径 用于保存训练过程中的模型文件。 对于在训练平台上运行的任务，该目录下的文件会上传至ufile中保存。
所以输出路径需要以 args.output\_dir为前缀。
模型输出的修改如下:
(L122)
<code>
torch.save(model.state_dict(), args.output_dir + '/mnist-param.pkl')
</code>
只要在代码中需要使用目录的地方，均按此方式修改，即可能够将其移植至uai-train平台。
最终在平台上运行时，data\_dir目录对应的是创建任务时填入的ufile输入路径，output\_dir对应的则是填入的ufile输出路径。

## 使用Docker镜像进行测试
因为UAI Train 平台使用Docker运行训练，因此可以使用Docker对训练代码进行测试。
UAI Train平台提供开源的Docker 打包工具，打包方法见[](uai-train/guide/pytorch/packing) 
打包完成后打包工具会自动生成GPU的镜像和CPU的镜像，命名如下：

  * **CPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>-cpu:<usr-def-tag>
  * **GPU镜像** uhub.ucloud.cn/<uhub-bucket>/<user-def-name>:<usr-def-tag>

### 使用CPU Docker 测试
打包工具会自动生成CPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for CPU local test: <docker run cmd> ）
直接执行命令即可

### 使用GPU Docker 测试
打包工具会自动生成GPU Docker的运行代码，可以在**标准输出**或**uaitrain\_cmd.txt**中找到命令（CMD for GPU local test: <docker run cmd> ）
直接执行命令即可

