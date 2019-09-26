{{indexmenu_n>4}}

# 本地环境准备

## 1.本地安装Docker
由于UCloud AI SDK依赖Docker环境来打包用户代码到UHub产品中，因此需要先安装Docker环境。
根据[[ai:uai-train:basic:docker]]的说明在本地准备Docker环境。
如果是CPU机器只需要安装普通Docker，如果是GPU机器需要安装NVidia-docker。

## 2.安装UFIle SDK
由于UCloud AI SDK依赖UFIle SDK来传输用户本地数据到UFile产品中，因此需要先安装UFile SDK 
<code>
wget http://sdk.ufile.ucloud.com.cn/python_sdk.tar.gz

tar zxvf python_sdk.tar.gz
cd ufile-python
sudo python setup.py install
</code>

## 3.安装UCloud AI SDK
用户需要通过通过UCloud AI SDK完成本地代码的一键打包、与云端UAI Train通过Python命令行进行指令控制，因此需要安装UCloud AI SDK。
<code>
git clone https://github.com/ucloud/uai-sdk

cd uai-sdk
sudo python setup.py install
</code>
UCloud AI SDK与UAI Train产品相关有两个目录：
<code>
uaitrain: uaitrain/arch 目录下包含了uai-train各种AI框架的SDK
uaitrain-tool:  包含了Docker镜像打包上传的工具
</code>

**Q&A**
遇到Please install setuptools问题？
需要安装setuptools，ubuntu可以使用如下命令：
<code>
sudo apt-get install python-setuptools
</code>

