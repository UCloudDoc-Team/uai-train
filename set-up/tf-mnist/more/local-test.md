{{indexmenu_n>24}}

===== 本地Mnist测试 =====
我们可以在云主机中测试Mnist镜像的正确性，当前的目录结构如下：
<code>
|_ /data/mnist
|  |_ code
|  |_ data
</code>

我们再创建一个output目录用来存储训练的输出：
<code>
|_ /data/mnist
|  |_ code
|  |_ data
|  |_ output
</code>

目前我们已经有tf-mnist-train-cpu:uaitrain这个CPU的Mnist训练镜像，我们就可以开始用CPU训练了。

==== 使用CPU训练 ====
我们执行如下命令来开始训练：
<code>
sudo docker run -it -v /data/mnist/data:/data/data -v /data/mnist/output:/data/output tf-mnist-train-cpu:uaitrain /bin/bash -c "cd /data && /usr/bin/python /data/mnist_summary.py --max_step=2000 --work_dir=/data --data_dir=/data/data --output_dir=/data/output --log_dir=/data/output/log"
</code>

** 数据挂载: ** \\
  * 我们通过 -v /data/mnist/data:/data/data 将数据从/data/mnist/data目录挂载到docker容器中的/data/data/目录下（注：AI训练平台也是挂载到/data/data/下)
  * 我们通过 -v /data/mnist/output:/data/output 将输出路径/data/mnist/output目录挂载到docker容器中的/data/output/目录下（注：AI训练平台也是挂载到/data/output/下) 
  * 在执行过程我们模拟添加了UAI Train系统会自动添加的参数： \-\-work\_dir=/data \-\-data\_dir=/data/data \-\-output\_dir=/data/output \-\-log\_dir=/data/output/log，用来测试代码的完备性