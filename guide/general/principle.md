{{indexmenu_n>1}}

## 设计原理

UAI
Train平台使用GPU主机来提供AI训练的基础算力，平台利用Docker容器技术来封装训练任务，并可以对接UFIle和UFS作为后端数据存储。
![](/images/general/ai_train综述.png)

其所涉及到的技术和产品包括包括：

1.  **Docker** 容器技术 [docker](/ai/uai-train/basic/docker)
2.  **UHub** UCloud Docker Hub [uhub](/ai/uai-train/basic/uhub)
3.  **UFile** UCloud 对象存储系统 [ufile](/ai/uai-train/basic/ufile)
4.  **UFS** UCloud 分布式存储系统 [ufs](/ai/uai-train/basic/ufs)
5.  **UDisk** UCloud 块存储系统 [udisk](/ai/uai-train/basic/udisk)

#### UAI Train执行的概念图

UAI Train平台在执行训练任务时，是通过将外部存储（UFile、UFS、UDisk）的数据映射到Docker容器中访问。整个执行逻辑如下：
![](/images/guide/train-general.png)

**其中有一些固定参数：**

1.  代码入口路径：/data
2.  输入数据路径：/data/data/
3.  输出数据路径：/data/output/

**注：** 固定参数为系统默认参数，无法更改。
