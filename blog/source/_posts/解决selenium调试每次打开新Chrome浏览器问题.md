---
title: 解决selenium调试每次打开新Chrome浏览器问题
date: 2024-03-14 16:36:36
tags: 测试
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/202403141638089.png
---

### Mac

配置chrome

```
open -e ~/.zshrc
```

添加

```
export PATH="/Applications/Google Chrome.app/Contents/MacOS:$PATH"
```

终端执行

```
source ~/.zshrc
```

然后执行

```
'Google Chrome' --remote-debugging-port=9222 --user-data-dir="/Users/xiaota/Documents/google/seleium"
或
Google\ Chrome --remote-debugging-port=9222 --user-data-dir="/Users/xiaota/Documents/google/seleium"
```

### Windows

添加环境变量 配置 chrome所在位置

终端执行

```
chrome.exe --remote-debugging-port=9222 --user-data-dir="D:google/selenium"
```



### 连接浏览器

```
from selenium import webdriver
 
options = webdriver.ChromeOptions()
 
options.add_experimental_option("debuggerAddress", "127.0.0.1:19222")
driver = webdriver.Chrome(options=option)
driver.get('https://www.baidu.com')
```

