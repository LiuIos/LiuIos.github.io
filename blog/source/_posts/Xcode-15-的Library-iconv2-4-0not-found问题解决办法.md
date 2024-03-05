---
title: Xcode 15 的Library "iconv2.4.0"not found问题解决办法
date: 2024-02-21 15:10:02
tags: iOS
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/202402211512531.png
---



### 1.首先在项目的targets 的build phases 移除iconv.2.4.0

![](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/202402211514655.png)

### 2.然后在link binary witch libraries 添加 libiconv.tbd 和 libiconv.2.tbd

![image-20240221151525420](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/image-20240221151525420.png)

### 3.在项目的targets 的Other Linker Flags 添加-ld64

