{{indexmenu_n>4}}

# 打包镜像
接下来我们需要使用UAI-SDK提供的打包工具来打包Mnist的训练镜像。

## 获取TensorFlow 镜像打包工具
TensorFlow镜像打包工具为 uai-sdk/uaitrain\_tool/tf/tf\_tool.py，我们将该工具也放入/data/mnist。
<code>
$ cd /data/mnist
$ cp ~/uai-sdk/uaitrain_tool/tf/tf_tool.py ./
</code>

目前的目录结构如下：
<code>
|_ /data/mnist
|  |_ code
|  |_ data
|  |_ tf_tool.py
</code>

## 打包Mnist镜像
我们使用tf\_tool.py 打包mnist镜像，具体的参数说明在[[ai:uai-train:guide:tensorflow:packing]]。
<code>
$ sudo python tf_tool.py pack \
                        --public_key=<YOUR_PUBLIC_KEY> \
			--private_key=<YOUR_PRIVATE_KEY> \
			--code_path=<CODE_PATH> \
			--mainfile_path=<MAIN_FUNC_FILE> \
			--uhub_username=<YOUR_UHUB_USER_NAME> \
			--uhub_password=<YOUR_UHUB_PASSWORD> \
			--uhub_registry=<YOUR_UHUB_REFDISTRY> \
			--uhub_imagename=<YOUR_UHUB_IMAGENAME> \
                        --internal_uhub=<true/false> \
			--ai_arch_v=<TF_ARCH> \
			--test_data_path=<LOCAL_DATA_PATH> \
			--test_output_path=<LOCAL_OUTPUT_PATH> \
			--train_params=<PARAMS>
</code>

### public_key & private_key
这里的公私钥是UCloud用户账号的唯一标识，可以依据[[ai:uai-train:basic:key]]的方法获取你账号的公私钥参数

### code_path
代码所在的路径，我们需要将该路径下的所有代码都拷贝至docker镜像中，通过 \-\-code\_path参数指定，本案例中参数为 \-\-code\_path=./code/

### mainfile_path
训练任务的入口程序，即main的入口python程序，在本案例中为 mainfile\_path.py

### uhub_username, uhub_password, uhub_registry
uhub的用户名密码为UCloud console图形界面登录时所用的邮箱和密码。在本案例中我们使用 uai_demo，实际操作中uhub\_registry就是uhub镜像库的名字。

### uhub_imagename
自定义的docker镜像名字，在本案例中我们使用tf-mnist-train

### internal_uhub
如果是使用ucloud云主机操作打包工具，则选择true，如果是使用公网执行打包操作，则选择false

### ai_arch_v 
训练框架的版本，这里我们选择tensorflow-1.1.0，这样就可以以tensorflow-1.1.0为基础镜像打包mnist的训练镜像。其他TensorFlow 基础镜像选择方法请参见[[ai:uai-train:guide:tensorflow:packing]]

### test_data_path
本地测试数据路径，本案例中我们填写 /data/mnist/data/

### test_output_path
本地测试输出路径，本案例中我们填写 /data/mnist/output/

### train_params
训练参数，我们设定--max\_step=2000

## 执行打包Mnist镜像命令
我们执行如下命令就可以生成mnist训练镜像。
<code>
$ sudo python tf_tool.py pack \
                        --public_key=<YOUR_PUBLIC_KEY> \
			--private_key=<YOUR_PRIVATE_KEY> \
			--code_path=./code/ \
			--mainfile_path=mnist_summary.py \
			--uhub_username=<YOUR_UHUB_USER_NAME> \
			--uhub_password=<YOUR_UHUB_PASSWORD> \
			--uhub_registry=uai_demo \
			--uhub_imagename=tf-mnist-train \
                        --internal_uhub=true \
			--ai_arch_v=tensorflow-1.1.0 \
			--test_data_path=/data/mnist/data \
			--test_output_path=/data/mnist/output \
			--train_params="--max_step=2000"
</code>

最终我们将获得两个镜像：
  - tf-mnist-train-cpu:uaitrain
  - uhub.service.ucloud.cn/uai_demo/tf-mnist-train:uaitrain

同时成功执行后，界面显示样例如下，会给出部署时所需的CMD命令以及本地测试的cmd命令:
<code>
CMD Used for deploying:
/data/mnist_summary.py --max_step=2000
CMD for CPU local test:
sudo docker run -it -v /data/mnist/data:/data/data -v /data/mnist/output:/data/output tf-mnist-train-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
CMD for GPU local test:
sudo nvidia-docker run -it -v /data/mnist/data:/data/data -v /data/mnist/output:/data/output uhub.service.ucloud.cn/uai_demo/tf-mnist-train:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>
  * **CMD Used for deploying**: 该输出的内容为创建训练任务时，**训练启动命令**框中需要填写的内容(参见[[ai:uai-train:set-up:how-to-use:create]])。可以直接复制黏贴到命令框中。
  * **CMD for CPU local test**: 该输出的内容为本地通过CPU来测试训练能否正常执行。在本地没有GPU的情况下可以使用该命令测试训练代码能否正常执行。
  * **CMD for GPU local test**：该输出的内容为本地通过GPU来测试训练能否正常执行。在本地有GPU的情况下可以使用该命令测试训练代码能否正常执行。本案例中我们的使用的是CPU镜像，因此无法进行GPU测试）

