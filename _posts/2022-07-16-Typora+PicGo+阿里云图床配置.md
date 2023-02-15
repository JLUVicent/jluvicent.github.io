---
layout: article
title: Typora+PicGo+阿里云OSS服务
tags: Typora
mathjax: true
key: A-2023-2-15
---

Typora+PicGo+阿里云OSS服务实现图床功能

<!--more-->

***

# Typora+PicGo+阿里云OSS服务

## Typora下载

下载Typora,目前Typora支持多平台安装，进入国内[官网](https://typoraio.cn/)，国外的太慢了，目前需要收费，89rmb即可拿下，若不想花钱，这里有[beta版](https://www.aliyundrive.com/s/26FApafqRFo),不过还是建议大家支持正版。（根据你的电脑选择具体版本），下载完先放着直接进行下一步。

![image-20220716132428443](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716132428443.png)

## 购买阿里云OSS服务

1.前提是你得有一个[阿里云](https://www.aliyun.com/)账号，没有的直接注册就行，然后选择对象存储OSS，如果找不到直接在搜索栏搜索OSS，点击折扣套餐或者立即购买。

![image-20220716132941868](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716132941868.png)

2.进入如下界面，资源包类型和地域不用管，存储包规格正常40GB完全够用了，购买时常自选，半年到5年不等，价格还是比较合适的，一年才9rmb，选择完成之后点击立即购买，然后付款完成。![image-20220716133240824](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716133240824.png)

3.创建Bucket,还是进入OSS对象存储，如果找不到就搜索，进入管理控制台。名称自定义，后面要用，地域选择离自己近的，读写权限改为公共读，其他不用动，点击确定。

![image-20220716133643641](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716133643641.png)

![image-20220716133819753](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716133819753.png)

![image-20220716133917940](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716133917940.png)

4.点击Bucket列表就可以看到你刚才创建的Bucket，点击概览

![image-20220716134206500](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134206500.png)

点击Bucket名称，进入Bucket管理

![image-20220716134251924](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134251924.png)

记住图中所指示的，我的是`vicent-picture-for-typora`和`oss-cn-beijing`。

![image-20220716134440259](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134440259.png)

点击文件管理，新建一个目录，我的是`img_for_typora/`。

![image-20220716134624472](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134624472.png)

5.创建AccessKey

鼠标放在右上角的图像上就出来了如下图，点击AccessKey管理

![image-20220716134847017](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134847017.png)

先点击继续使用AccessKey，然后再点击创建AccessKey。

![image-20220716135031692](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716135031692.png)

创建完成后，复制AccessKey ID和AccessKey Secret。

![image-20220716151419938](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716151419938.png)

## PicGo下载

可以去[官方](https://github.com/Molunerfinn/PicGo/releases/tag/v2.3.0)下载，选择对应的版本，我的是windows64,选择如下图。

如果github访问不了，可以自取[安装包](https://www.aliyundrive.com/s/ViQCSDpgcHL)

![image-20220716135416220](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716135416220.png)

直接点击安装，成功界面如下

![image-20220716135601939](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716135601939.png)

点击图床设置，然后点击阿里云OSS，

![image-20220716135646219](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716135646219.png)

这里的设定KeyId将之前的AccessKey ID复制过来

设定KeySecret将之前的AccessKey Secret复制过来

设定存储空间名就是下图中的1，之前2.4的设置

确认存储区域就是下图中的2，之前2.4的设置

存储路径为之前新建目录名

然后点击确定，设置为默认图床

![image-20220716135754708](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716135754708.png)

![image-20220716134440259](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716134440259.png)

## Typora配置

点击`文件->偏好设置->图像`按照如下图进行设置，其中PicGo路径为之前安装PicGo的路径

![image-20220716140530117](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716140530117.png)

至此所有的步骤都已经配置完成，可以直接再Typora中将图片上传到图床了。

## 注意

此时上传图片或者点击验证图片上传选项会报错`Failed to fetch`，如下图：

![image-20220716140916800](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716140916800.png)

![image-20220716140951180](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716140951180.png)

报错原因：如上图所示，Typora端口使用的是36677，而PicGo使用的端口是36678，此时只需要将PicGo的端口号改为36677即可，如下图所示：

打开PicGo选择PicGo设置，设置监听端口为36677，问题解决。此时你如果点击验证图片上传选项仍然报错，没有关系，图片已经能够上传了，这可能是一个typora的bug。

![image-20220716141120231](https://vicent-picture-for-typora.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20220716141120231.png)