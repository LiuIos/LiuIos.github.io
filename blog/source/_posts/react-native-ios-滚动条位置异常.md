---
title: react native ios 滚动条位置异常
date: 2022-01-13 10:14:38
tags: react-native
categories: 技术
toc: false
---

> React native ScrollView FlatList 滚动条位置异常、错位

![image-20220113144619480](https://cdn.jsdelivr.net/gh/LiuIos/picBed/image-20220113144619480.png)

### 解决方法

1. ScrollView 增加属性

```
scrollIndicatorInsets={{ right: 1 }}
```

2. 在根目录 index.js 中添加

```
import { ScrollView } from 'react-native';

ScrollView.defaultProps = ScrollView.defaultProps || {};
ScrollView.defaultProps.automaticallyAdjustsScrollIndicatorInsets = false;
```

参考

[https://github.com/facebook/react-native/issues/26610](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Ffacebook%2Freact-native%2Fissues%2F26610)
