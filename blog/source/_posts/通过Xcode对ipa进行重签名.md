---
title: 通过Xcode对ipa进行重签名
date: 2023-04-28 10:07:39
tags: 
  - 破解
  - ipa
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/202304281016264.png
toc: false
---

### 1. 首先新建一个Xcode工程在工程目录下新建一个APP的文件夹

![image-20230428101913826](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/image-20230428101913826.png)



### 2.在工程中新增一个脚本运行

![image-20230428102122114](https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/image-20230428102122114.png)

脚本如下

```ruby
ASSETS_PATH="${SRCROOT}/APP"

TEMP_PATH="${SRCROOT}/temp"

TARGET_IPA_PATH="${ASSETS_PATH}/*.ipa"


#删除temp文件夹下的内容 然后新建
rm -rf "${SRCROOT}/temp"
mkdir -p "${SRCROOT}/temp"


#-----------------------------
#1.解压ipa 到temp下
unzip -oqq "$TARGET_IPA_PATH" -d "$TEMP_PATH"

#拿到解压的临时的app路径
TEMP_APP_PATH=$(set -- "$TEMP_PATH/Payload/"*.app;echo "$1")
#echo "路径是:$TEMP_APP_PATH"

#-----------------------------
#2. 将解压出来的app拷贝到工程下
#BUILT_PRODUCTS_DIR 工程生成包的路径
#TARGET_NAME
TARGET_APP_PATH="$BUILT_PRODUCTS_DIR/$TARGET_NAME.app"
#echo "app路径是:$TARGET_IPA_PATH"

rm -rf "$TARGET_APP_PATH"
mkdir -p "$TARGET_APP_PATH"
cp -rf "$TEMP_APP_PATH/" "$TARGET_APP_PATH"


#-----------------------------
#3. 删除extension和watch
rm -rf "$TARGET_APP_PATH/PlugIns"
rm -rf "$TARGET_APP_PATH/watch"

#-----------------------------
#4. 修改 info.plist

# 设置 "Set : KEY Value" "目标文件路径"

#/usr/libexec CFBundleIdentifier

# /usr/libexec/PlistBuddy -c "Set :CFBundleIdentifier $PRODUCT_BUNDLE_IDENTIFIER" "$TARGET_APP_PATH/Info.plist"

# 删除设备限制
/usr/libexec/PlistBuddy -c "Delete :UISupportedDevices" "$TARGET_APP_PATH/Info.plist"


#-----------------------------
#5. 给可执行文件上执行权限
#拿到macho 文件路径
APP_BINARY=`plutil -convert xml1 -o - $TARGET_APP_PATH/Info.plist|grep -A1 Exec|tail -n1|cut -f2 -d\>|cut -f1 -d\<`
#上权限
chmod +x "$TARGET_APP_PATH/$APP_BINARY"


#-----------------------------
#6.重签名第三方app 第三方的frameworks
TARGET_APP_FRAMEWORKS_PATH="$TARGET_APP_PATH/Frameworks"
if [ -d "$TARGET_APP_FRAMEWORKS_PATH" ] ; then
#echo "六六六:$FRAMEWORK";
for FRAMEWORK in "$TARGET_APP_FRAMEWORKS_PATH/"*
do
#echo "包的路径:$FRAMEWORK"
#签名
/usr/bin/codesign --force --sign "$EXPANDED_CODE_SIGN_IDENTITY" "$FRAMEWORK"
    done
fi
```
