---
title: markerdown-高效上传图片工具PicGo
date: 2021-03-24 16:05:38
tags:
- markerdown
- 原创
categories:
- 开发工具
cover: https://cdn.jsdelivr.net/gh/tangyuxian/blog_image@master/post/markerdown.jpg
coverWidth: 1200
coverHeight: 320
---
在使用Typora时可随时便捷上传图片
<!--more-->

## 一 应用场景

在编写markerdown文档时,需要插入图片,因文档最终上传到云端,所以需要将网络图片地址插入其中;

**PicGo**工具在Typora中内置并集成,可使用该插件将本地或其它网络图片直接上传到云端

## 二 下载地址

官方网站:[https://molunerfinn.com/PicGo/](https://molunerfinn.com/PicGo/)

下载地址:[https://github.com/Molunerfinn/picgo/releases](https://github.com/Molunerfinn/picgo/releases)

## 三 使用方式

#### 1下载PicGo并配置

​	下载安装完毕后,参照[文档](https://picgo.github.io/PicGo-Doc/zh/guide/config.html)进行配置,如图是糖糖的配置项

​	![image-20210324162149788](https://cdn.jsdelivr.net/gh/tangyuxian/blog_image@latest/PicGo/20210324162152.png)

这里推荐[github配置](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#github图床),设定的自定义域名为**jsdelivr**可享有免费的CDN加速

https://cdn.jsdelivr.net/gh/`[github用户名]`/`[仓库名]`@`[版本分支]`

例如:https://cdn.jsdelivr.net/gh/tangyuxian/blog_image@latest

#### 2 在Typora中配置

<img src="https://cdn.jsdelivr.net/gh/tangyuxian/blog_image@latest/PicGo/20210324162544.png" alt="image-20210324162542890"  />

在`文件`->`偏好设置`->`图像`中配置`上传服务设定`

按照图中配置即可

插件配置方式可参照[官方文档](https://support.typora.io/Upload-Image/)进行配置

#### 3 使用

日常编辑时,可随时截图,在Typora处粘贴,右键图片便可看到`上传图片`功能,上传成功后会自动替换本地图片路径;

在网上复制文章中,会夹带网络地址的图片,部分网络图片因为**防盗链**技术,无法在自己的网站上显示这些图,同样可以在图片上右键选择`上传图片`,将图片上传到自己的服务源,一劳永逸
