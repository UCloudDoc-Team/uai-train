

# FAQ

## 1. 实时日志刷新不及时

  * 可能是网络问题，可以通过刷新尝试
  * 可能由于docker执行的python程序自动buffer stdout导致，可以通过在训练启动命令 处增加 -u 参数解决：

<code>
例如：

​    /data/mnist_summary.py xxx
修改为：
​    -u /data/mnist_summary.py xxx
</code>

## 2. 启动后遇到 /usr/bin/python: can't open file 'python': [Errno 2] No such file or directory

UAI-Train平台执行代码时会自动添加 /usr/bin/python 命令，因此用户仅需要直接填写执行程序和启动参数即可，例如 mnist.py \-\-learning\_rate=0.01

## 3. 图像渲染是否能使用UAI-Train？

符合如下条件的渲染任务，也可以运行在UAI-Train上，享受其弹性扩展和分布式调度的便利：

* 渲染任务基于Linux操作系统
* NVidia显卡驱动用的是cuda
* 可以通过命令行CLI启动和调试渲染任务，无需图形化界面操作

