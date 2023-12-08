---
title: Xcode真机运行报错：Failed to prepare the device for development解决方法
date: 2023-04-28 10:25:17
tags: 
  - iOS
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/202304281039628.png
toc: false
---

>本文讲解了在Xcode不支持iOS系统版本时如何通过增加Xcode系统支持来解决问题。当出现报错“Failed to prepare the device for development. This operation can fail if the version of the OS on the device is incompatible with the installed version of Xcode. You may also need to restart your Mac and device in order to correctly detect compatibility.”时，首先尝试重启iOS设备，之后查看Xcode版本是否支持当前手机系统。如果不支持，可以通过进入iOSDeviceSupport项目（Gitee或Github）下载需要支持的系统文件，新建一个文件夹，然后将解压好的内容全部粘贴进去，最后再真机运行即可。

### 当遇到此报错

Failed to prepare the device for development. This operation can fail if the version of the OS on the device is incompatible with the installed version of Xcode. You may also need to restart your Mac and device in order to correctly detect compatibility.

首先你应该做的是尝试重启你的iOS设备。

重启连不上之后可以查看一下你的Xcode版本是否支持你现在的手机系统。

------

### 增加Xcode系统支持
进入[ iOSDeviceSupport ](https://gitee.com/Han0/iOSDeviceSupport)（或者Github在[这里](https://github.com/JinjunHan/iOSDeviceSupport)）

下载你需要支持的系统文件

![image-20230428103219546](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/image-20230428103219546.png)

然后再打开终端，输入

```sh
open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
```

然后把解压的文件夹放进去

![image-20230428103527084](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/image-20230428103527084.png)
