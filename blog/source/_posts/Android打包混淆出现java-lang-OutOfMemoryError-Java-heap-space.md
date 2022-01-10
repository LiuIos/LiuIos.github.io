---
title: "Android打包混淆出现java.lang.OutOfMemoryError: Java heap space"
date: 2021-09-08 10:56:42
tags:
  - Android
categories: 技术
toc: false
---

配置 gradle.properties

```
org.gradle.jvmargs=-Xmx4096m -XX:MaxPermSize=4096m -XX:+HeapDumpOnOutOfMemoryError
org.gradle.daemon=true
org.gradle.parallel=true
org.gradle.configureondemand=true
```
