

# url-查询TensorBoard URL
使用该命令可以查询训练任务的详细信息（**仅针对tensorflow框架**），工具在uaitrain\_tool下面：
<code>
python tf/tf_tool.py url --args
</code>

## 命令参数说明
| 参数 | 说明 | 是否必需 | 默认值 |
| ---- | ---- | -------- | ------ |
| public\_key   | 用户的公钥                | 是      |  无           |
| private\_key  | 用户的私钥                | 是      |  无           |
| job\_id       | 任务ID（Create 命令的返回值）  | 是      |  无           |
| project\_id   | 项目组id                | 否      |  用户默认项目组     |
| region        | 训练任务执行所在地域           | 否      |  默认地域（北京二）   |
| zone          | 训练任务所在可用区            | 否      |  默认可用区  |

## 返回值说明
**返回tensorboard的url信息**

## 命令操作案例
查看tensorflow任务的tensorboard的url地址
<code>
python tf_tool.py url --public_key='<PUB_KEY>' \

​    --private_key='<PRI_KEY>' \
​    --job_id=’<JOB_ID>’
</code>