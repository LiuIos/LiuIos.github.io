---
title: iOS 启动图尺寸适配
date: 2021-09-08 10:02:08
tags:
  - iOS
  - Android
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/20210908100948.png
toc: false
---

> 在 iOS 开发早期，启动图适配，可以通过自定义 LaunchImage 通过设置多张图片来实现通过尺寸的适配，2020 年 4 月开始，所有使用 iOS 13 SDK 的 App 都必须提供 LaunchScreen 如果使用 LaunchScreen 一张图来适配所有尺寸的 iPhone 是一定不够的，不同程度的拉伸是不可避免的，解决办法 👇

**方法一**

LaunchScreen 中写自定义布局，把 LaunchScreen 当做一个页面，里面不仅仅可以放图片，也来用来放其他 view,这样，重新布局，来实现我们想要的启动页就很容易实现了，这种也是官方推荐的方法

**方法二**

仍然使用类似于 LaunchImage 的方法来实现，通过配置多张图片的方式，来达到多图适配的问题

1. 在 Assets.xcasset 中新建一个 Image Set，并配置
2. 拷贝如下 xml 内容到 Contents.json 中,并配置尺寸的图片到指定位置，设置只支持 iPhone 图片

```json
{
  "images": [
    {
      "filename": "1125x2436.png",
      "idiom": "iphone",
      "scale": "1x"
    },
    {
      "filename": "640x960.png",
      "idiom": "iphone",
      "scale": "2x"
    },
    {
      "filename": "1125x2436.png",
      "idiom": "iphone",
      "scale": "3x"
    },
    {
      "filename": "640x1136.png",
      "idiom": "iphone",
      "scale": "1x",
      "subtype": "retina4"
    },
    {
      "filename": "640x1136.png",
      "idiom": "iphone",
      "scale": "2x",
      "subtype": "retina4"
    },
    {
      "filename": "640x1136.png",
      "idiom": "iphone",
      "scale": "3x",
      "subtype": "retina4"
    },
    {
      "filename": "1242x2208.png",
      "idiom": "iphone",
      "scale": "3x",
      "subtype": "736h"
    },
    {
      "filename": "750x1334.png",
      "idiom": "iphone",
      "scale": "2x",
      "subtype": "667h"
    },
    {
      "filename": "1125x2436.png",
      "idiom": "iphone",
      "scale": "3x",
      "subtype": "2436h"
    },
    {
      "filename": "1242x2688.png",
      "idiom": "iphone",
      "scale": "3x",
      "subtype": "2688h"
    },
    {
      "filename": "828x1792.png",
      "idiom": "iphone",
      "scale": "2x",
      "subtype": "1792h"
    }
  ],
  "info": {
    "author": "xcode",
    "version": 1
  }
}
```

3.设置好后，把图片加入到 LaunchImage 中，设置好约束，并把 contentMode 设置为`Scale to Fill`

删除 app 重新运行就能看到效果了。

记录一下 Android 启动图尺寸

```
android 启动图尺寸

mdpi —— 360x640px
hdpi —— 540x960px
xhdpi —— 720x1280px
xxhdpi —— 1080x1920px
xxxhdpi —— 1440x2560px
```
