{{indexmenu_n>12}}



# 自定义镜像打包（Pack）
使用该命令可以创建新的训练任务，所有工具都在uaitrain\_tool下面：
<code>
python base_tool.py pack --args
</code>

<code>
sudo python base_tool.py pack [-h] 
                        --code_path CODE_PATH 
                        --mainfile_path MAINFILE_PATH
                        --uhub_username UHUB_USERNAME
                        --uhub_password UHUB_PASSWORD 
                        --uhub_registry UHUB_REGISTRY
                        --uhub_imagename UHUB_IMAGENAME
                        [--uhub_imagetag UHUB_IMAGETAG]
                        [--internal_uhub false/true]
                        --test_data_path TEST_DATA_PATH
                        --test_output_path TEST_OUTPUT_PATH
                        --train_params TRAIN_PARAMS
                        --self_img YOUR_SOURCE_INAGE
</code>

| 参数 | 说明 | 是否必需 |
| ---- | ---- | -------- |
| code\_path          | 训练任务所需代码路，以/结尾(请填写相对pytorch_tool.py的相对路径，本例中为./code/)                                                  | 是              |
| mainfile\_path      | 训练主文件（有main函数），不须包含路径，但须包含后缀名（本例中为：mnist.py）                                                           | 是              |
| uhub\_username      | uhub账号(即UCloud账号)                                                                                      | 是              |
| uhub\_password      | uhub密码(即UCloud账号密码)                                                                                    | 是              |
| uhub\_registry      | uhub镜像仓库名称，不支持大写字母                                                                                     | 是              |
| uhub\_imagename     | 生成镜像名称，不支持大写字母                                                                                         | 是              |
| uhub\_imagetag      | 生成镜像tag，不支持大写字母                                                                                        | 否，默认为uaitrain  |
| internal\_uhub      | 告诉打包程序是否使用uhub的UCloud内网地址，如果true则使用uhub.service.ucloud.cn，false则使用uhub.ucloud.cn，使用内网地址可以提高镜像拉取和上传的速度  | 否，默认为false     |
| **self\_img** | **自定义源Docker Image的名字** | 是 |
| test\_data\_path    | 本地测试环境数据输入路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| test\_output\_path  | 本地测试环境训练输出路径（建议用绝对路径），生成的本地测试脚本将通过挂载方式接入到docker镜像中                                                     | 是              |
| train\_params       | 训练所需参数                                                                                                 | 否              |

## 命令范例
<code>
sudo python base_tool.py pack \
			--code_path=./code/ \
			--mainfile_path=mnist_summary.py \
			--uhub_username=<YOUR_UHUB_USER_NAME> \
			--uhub_password=<YOUR_UHUB_PASSWORD> \
			--uhub_registry=<YOUR_UHUB_REFDISTRY> \
			--uhub_imagename=<YOUR_UHUB_IMAGENAME> \
                        --internal_uhub=true \
			--test_data_path=/data/test/data \
			--test_output_path=/data/test/output \
			--train_params="--max_step=2000" \
                        --self_img=<YOUR_BASE_IMAGE_NAME>
</code>

