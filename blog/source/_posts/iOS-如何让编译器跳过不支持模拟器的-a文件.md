---
title: iOS 如何让编译器跳过不支持模拟器的.a文件
date: 2022-01-18 14:27:55
tags: iOS
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/20220118143352.png
toc: false
---

> 我们在开发中经常会接入第三方的静态库.a 文件，有的静态库文件支持的架构比较多 x86、arm64、arm7s、arm7这样我们编译的时候不会出错。但是支持的架构越多最后生成的ipa包就越大，比如x86的架构，生产环境根本用不到，许多第三方给出的.a文件就不会包含这个架构。这样就只能真机运行测试了。那我们想用模拟器测试怎么办呢？下面给出两种解决办法。

## 规避法

```
#if TARGET_IPHONE_SIMULATOR
    
#else
  //调用第三方.a文件中的方法
#endif
```

## 补全法

1. 新建一个同名的工程
2. 把第三方库的头文件都拷贝进去
3. 把所有借口都使用假函数实现一遍，然后编译出模拟器版本
4. 把第三方的静态库lib1.a和我们编译出来的lib2.a合并``lipo -create lib1.a lib2.a -output libXATA.a``

