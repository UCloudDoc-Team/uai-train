

# 以网盘方式访问US3

US3支持以Fuse方式将US3空间挂载到用户自己的主机上，以网盘方式访问。
![](/ai/uai-train/images/basic/ufile/fuse_macos.png)

## 1. 环境准备
安装Fuse基础安装包，注意s3fs的版本需在1.83以上

1. Debian 9或Ubuntu 16.04以上 

不能用apt-get自动安装，因其版本仅为1.82
<code bash>
sudo apt-get install s3fs
</code>

可以用如下命令查询版本号：
<code bash>
s3fs --version
</code>

正确的安装方式为通过GitHub下载
<code bash>
wget https://github.com/s3fs-fuse/s3fs-fuse/archive/v1.83.zip
</code>

然后解压安装
<code bash>
unzip v1.83.zip
cd s3fs-fuse-1.83/
./autogen.sh
./configure
make
sudo make install
</code>

2. RHEL或CentOS 7以上

可以用yum自动安装，当前版本为1.85
<code bash>
sudo yum install epel-release
sudo yum install s3fs-fuse
</code>

3. MacOS

<code bash>
brew cask install osxfuse
brew install s3fs
</code>

## 2. 配置密钥文件
从UCloud控制台界面获取公钥、私钥，具体参见[](uai-train/basic/key) 
<code bash>
echo 公钥:私钥 > ${HOME}/.passwd-s3fs

chmod 600 ${HOME}/.passwd-s3fs
</code>

## 3. 挂载US3存储空间

1. 建立US3挂载文件路径${LocalMountPath} 

2. 获取[](uai-train/basic/ufile/create)中创建的存储空间名称${UFileBucketName}。

  **注意**：空间名称不带域名后缀，如US3空间名称显示为bbh.cn-bj.ufileos.com，则${UFileBucketName}=bbh

3. 根据US3存储空间所在区域、本地服务器是否在UCloud内网，从[](ufile/s3)中获取${UFileS3URL}地址 

4. 密钥文件地址${HOME}/.passwd-s3fs与步骤“2. 配置密钥文件”中匹配 

<code bash>
  sudo s3fs ${UFileBucketName} ${LocalMountPath} -ourl=http://${UFileS3URL} -o passwd_file=${HOME}/.passwd-s3fs -o dbglevel=info -o curldbg,use_path_request_style,allow_other -o retries=1 -o multipart_size=8 -o multireq_max=8 -o parallel_count=32
</code>

若US3 bucket为 s3fs-test，位于北京，主机挂载路径为 /data/vs3fs，则示例命令为：

<code bash>
s3fs s3fs-test /data/vs3fs -o url=http://internal.s3-cn-bj.ufileos.com -o passwd_file=~/.passwd-s3fs -o dbglevel=info -o curldbg,use_path_request_style,allow_other -o retries=1 -o multipart_size=8 -o multireq_max=8 -o parallel_count=32
</code>

挂载成功后效果如图：
![](/ai/uai-train/images/basic/ufile/fuse_df.png)

## 4. 上传下载文件
挂载US3存储空间后，可以像使用本地文件夹一样使用US3存储空间。

1. 拷贝文件到${LocalMountPath}，即是上传文件 
2. 将文件从${LocalMountPath}拷贝到其他路径，即是下载文件 
1. 不符合Linux文件路径规范的路径，可以在US3管理控制台看到，但不会在Fuse挂载的${LocalMountPath}下显示 
2. Fuse使用枚举文件清单会比较缓慢，建议直接使用指定到具体文件的命令, 如vim,cp,rm指定具体文件。

## 5. 卸载US3存储空间
<code bash>
sudo umount ${LocalMountPath}
</code>

