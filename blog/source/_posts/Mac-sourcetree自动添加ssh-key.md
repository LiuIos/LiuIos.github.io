---
title: Mac sourcetree自动添加ssh key
date: 2021-09-07 18:23:12
tags: SourceTree
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/20210907183411.png
toc: false
---

```
生成sshkey
执行ssh-add ~/.ssh/id_rsa 将sshkey添加到sourceTree
执行ssh-add -K ~/.ssh/id_rsa 将sshkey添加到钥匙串
cd 到 .ssh目录下, 用touch config命令创建config文件
执行open config, 打开config文件.
输入下面的配置内容, 保存·config文件
```

config

```
Host *
   UseKeychain yes
   AddKeysToAgent yes
   IdentityFile ~/.ssh/id_rsa
   IdentityFile ~/.ssh/github_rsa
```
