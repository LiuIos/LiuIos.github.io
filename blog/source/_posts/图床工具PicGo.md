---
title: 图床工具PicGo
date: 2022-01-05 15:26:09
tags:
  - 工具
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed/20220105152934.png
toc: true
---

> 使用 Picgo+Github 搭建个人图床，为本地图片添加外部链接。

## 1. 创建 GitHub 仓库

> 注意事项：仓库类型要选择 public，否则访问不到图片

![](https://cdn.jsdelivr.net/gh/LiuIos/picBed/20220105155417.png)

![](https://cdn.jsdelivr.net/gh/LiuIos/picBed/20220105155800.png)

## 2.获取 token

1. 创建完成后进入，选择右上角的 Settings

![image-20220105160218992](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105160218992.png)

2. 选择 Developer settings

![image-20220105160501024](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105160501024.png)

3.选择 Personal access tokens，然后点击 Generate new token

![image-20220105160724298](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105160724298.png)

4. 填写一下 Note，过期时间选择无期限，并勾选 repo 即可，拉到页面最下方确认提交

![image-20220105161002173](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105161002173.png)

5. 创建完成后，即可看到 token。这个值一定要保存下来，因为只显示一次。

   ![](https://cdn.jsdelivr.net/gh/LiuIos/picBed/20220105161311.png)

## 3. PicGo 设置

1.[下载 Picgo](https://molunerfinn.com/PicGo/)，选择你所需的版本

![image-20220105163228252](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105163228252.png)

2. 配置 GitHub 图床

   > 仓库名、分支、token

   ![image-20220105163548415](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105163548415.png)

3. 配置好后，就能上传图片了。

   ![image-20220105163837173](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105163837173.png)

## 4.搭配 CDN 使用

> 使用 GitHub 仓库作为图床，存在的问题是国内访问 github 的速度很慢，可以利用 [jsDelivr CDN](https://www.jsdelivr.com/?docs=gh) 来加速访问。jsDelivr 是一个免费开源的 CDN 解决方案，该平台是首个打通中国大陆与海外的免费 CDN 服务，拥有中国政府颁发的 ICP 许可证，无须担心中国防火墙问题而影响使用。使用 jsDelivr 加速访问，需要将自定义域名设置为`https://cdn.jsdelivr.net/gh/用户名/图床仓库名/`。

![image-20220105164501818](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105164501818.png)

加上 cdn 后图片访问速度飞快。

测试： https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105164501818.png

## 5.Typora 配置图床

Typora 是最好用的 Markdown 编辑器，还可以搭配图床一起使用，非常 nice ～

1. PicGO 设置 Server

![image-20220105165803914](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105165803914.png)

![image-20220105165845304](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105165845304.png)

2. Typora 设置

   ![image-20220105165929721](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105165929721.png)

右键上传就行了。

![image-20220105170127016](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220105170127016.png)
