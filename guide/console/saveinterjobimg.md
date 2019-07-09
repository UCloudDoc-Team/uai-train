{{indexmenu_n>7}}

## 保存交互式训练镜像

Step1: 进入UAI-Train产品页面  
1\.
登录UCloud官方网站，点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取Ucloud产品列表。选择“人工智能”列表下的“AI训练服务
UAI-Train”选项，进入UAI-Train产品页面。  
![](/images/set-up/how-to-use/ai产品.jpg)

-----

Step2: 保存交互式训练镜像

1\. UAI-Train为用户提供了两种保存镜像任务的入口。  
**说明：必须在“执行中”的任务才能保存交互式训练镜像**  
方法一：在产品页面的训练任务列表上找到目标任务，选择操作列表中的“保存镜像”选项。  
![](/images/use/saveinterjobimage0.png)  
方法二：点击目标任务，进入任务概览页面，点击“保存镜像”选项。  
![](/images/use/saveinterjobimage1.png)

2\. 填写弹出框信息，选择“确定”  
![](/images/use/saveinterjobimage2.png)

3\. 弹出提示框，选择“确定”  
**说明1：确定后要在镜像保存过程中对Jupyter进行操作**  
**说明2：填写“代码路径”，“输入路径”，“结果路径”的目录部分，不会被镜像保存，只会保存在云存储产品上**  
![](/images/use/saveinterjobimage3.png)

-----

Step3: 查看保存镜像

1\.
登录UCloud官方网站，点击右上角“控制台”选项，进入控制台页面。控制台页面显示账户的基本信息，点击左上角“产品与服务”选项，获取Ucloud产品列表。选择“容器服务”列表下的“容器镜像库
UHub”选项，进入Uhub产品页面。  
![](/images/use/saveinterjobimage4.png)

2\. 输入保存镜像的名称查询，等待直到镜像出现
**说明1：等待时间与镜像变化程度有关，如果安装新增了很多内容，会导致镜像传输较慢，请耐心等待**  
![](/images/use/saveinterjobimage5.png)
