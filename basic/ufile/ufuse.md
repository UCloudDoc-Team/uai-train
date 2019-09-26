{{indexmenu_n>2}}

# 以网盘方式访问UFile

UFile支持以Fuse方式将UFile空间挂载到用户自己的电脑上，以网盘方式访问。
{{:ai:uai-train:basic:ufile:fuse_macos.png?600|}}

## 1. 环境准备
安装Fuse基础安装包

1. Debian 9或Ubuntu 16.04以上 
<code bash>
sudo apt-get install s3fs
</code>
2. RHEL或CentOS 7以上
<code bash>
sudo yum install epel-release
sudo yum install s3fs-fuse
</code>
2. MacOS
<code bash>
brew cask install osxfuse
brew install s3fs
</code>

## 2. 配置密钥文件
从UCloud控制台界面获取公钥、私钥，具体参见[[ai:uai-train:basic:key]] 
<code bash>
echo 公钥:私钥 > ${HOME}/.passwd-s3fs

chmod 600 ${HOME}/.passwd-s3fs
</code>

## 3. 挂载UFile存储空间

1. 建立UFile挂载文件路径${LocalMountPath} 
2. 获取[[ai:uai-train:basic:ufile:create]]中创建的存储空间名称${UFileBucketName}。
//注意：空间名称不带域名后缀，如UFile空间名称显示为bbh.cn-bj.ufileos.com，则${UFileBucketName}=bbh
3. 根据UFile存储空间所在区域、本地服务器是否在UCloud内网，从[[storage_cdn:ufile:s3]]中获取${UFileS3URL}地址 
4. 密钥文件地址${HOME}/.passwd-s3fs与步骤“2. 配置密钥文件”中匹配 
<code bash>
sudo s3fs ${UFileBucketName} ${LocalMountPath} -ourl=http://${UFileS3URL} -o passwd_file=${HOME}/.passwd-s3fs -o dbglevel=info -o curldbg,use_path_request_style,allow_other -o retries=1 -o multipart_size=8 -o multireq_max=8 -o parallel_count=32
</code>
挂载成功后效果如图：
{{:ai:uai-train:basic:ufile:fuse_df.png?600|}}

## 4. 上传下载文件
挂载UFile存储空间后，可以像使用本地文件夹一样使用UFile存储空间。

1. 拷贝文件到${LocalMountPath}，即是上传文件 
2. 将文件从${LocalMountPath}拷贝到其他路径，即是下载文件 
注意： 
1. 路径不符合Linux文件路径规范的路径，可以再UFile管理控制台看到，但不会在Fuse挂载的${LocalMountPath}下显示 
2. Fuse使用枚举文件清单会比较缓慢，建议直接使用指定到具体文件的命令, 如vim,cp,rm指定具体文件。

## 5. 卸载UFile存储空间
<code bash>
sudo umount ${LocalMountPath}
</code>

