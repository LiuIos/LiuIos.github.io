---
title: react-native修改第三方包
tags: react-native
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed/20210907104054.png
toc: false
---

使用第三方包的时候，经常需要修改内部源码来解决 bug 和实现效果，然而每次 yarn、npm install 之后，修改的代码又会被覆盖，让人抓狂，下面介绍一个好用的方法 👇

使用工具：[patch-package](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fds300%2Fpatch-package)

### 用法

修改 package.json,添加` "postinstall": "patch-package"`

```json
"scripts": {
    "android": "react-native run-android",
    "ios": "react-native run-ios",
    "start": "react-native start",
    "test": "jest",
    "lint": "eslint .",
    "postinstall": "patch-package"
  },
```

执行 `yarn add patch-package postinstall-postinstall -D`

修改第三方库后，

```
npx patch-package [package-name]
或
yarn patch-package [package-name]
```

应用补丁

```bash
yarn patch-package
```
