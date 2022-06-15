# 在 Typora 中基于阿里云 OSS + PicGo 搭建图床 


<br>

图床可以理解成是一个在互联网上专门用来存放图片的场所，每一张图片都可以通过 URL 被其他人使用

<!--more-->

### 必备物料

- Typora
- PicGo
- 阿里云 OSS 服务





### 开通阿里云 OSS 服务

1. 登录阿里云官网，在产品中搜索 OSS，开通对象存储 OSS 服务

   ![image-20220601173210692](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173210692.png)

   ![image-20220601173314795](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173314795.png)

   ![image-20220601173408389](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173408389.png)

2. 进入管理控制台，购买对象存储 OSS 资源包

   > 根据个人情况选择对应套餐即可。这里我选择一年时长，其余都保持默认配置。

   ![image-20220601173450678](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173450678.png)

   ![image-20220601173525703](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173525703.png)

   ![image-20220601173759752](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601173759752.png)

3. 创建 Bucket

   进入控制台，选择左侧的 `Bucket 列表`，点击 `创建 Bucket`

   ![image-20220601174226899](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601174226899.png)

   填写各选项，点击确定即可

   ![image-20220601174532282](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601174532282.png)

4. 创建用户 AccessKey

   点击右上角头像，选择 `AccessKey 管理`

   ![image-20220601174742318](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601174742318.png)

   ![image-20220601174845231](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601174845231.png)

   ![image-20220601174919503](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601174919503.png)

   ![image-20220601175040240](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601175040240.png)

   ![image-20220601175417860](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601175417860.png)

   ![image-20220601175742899](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601175742899.png)

   ![image-20220601175854228](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601175854228.png)

   ![image-20220601175916902](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601175916902.png)

   创建好 AccessKey 后妥善保存



### PicGo

1. 打开 Typora `设置`-`通用`，勾选 `开启调试模式`

   ![image-20220601180307682](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601180307682.png)

2. 按下图设置`图像`选项![image-20220601180501134](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601180501134.png)

3. 下载 [PicGo](https://molunerfinn.com/PicGo/)

4. 打开 PicGo ，选择`图床设置`-`阿里云OSS`进行配置

   ![image-20220601181315740](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601181315740.png)

   ![image-20220601181435646](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601181435646.png)

5. 验证

   打开 Typora 的偏好设置，点击`图像`-` 验证图片上传选项`，查看返回信息。

   ![image-20220601182219563](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601182219563.png)

   可以看到，PicGo 上传图片到阿里云 OSS 的 Bucket 中是成功的，设置完成。

   ![image-20220601182323327](https://menah3m-image-bucket.oss-cn-chengdu.aliyuncs.com/img/image-20220601182323327.png)


