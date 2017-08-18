---
title: 如何反编译 APK 包和重打包
date: 2017-08-03 16:47:49
categories: Android
description: 以前反编译都是使用别人写好的工具，好一阵子没用了，发现工具找不到，于是就把原来捣鼓的 apktool 重新拿出来实践一番并记录下来。
tags: [Android, APK, Decompile]
---

## 背景

因为找不到原来的版本节点了，现在只有一个 APK 包，包有点久了，难以追溯了，于是只能把包拆开再重新打包一下了。

那么问题来，虽然以前有研究过一阵子反编译，不过年代久远了，都忘记是什么回事了，只能去找找原来的代码回来温习一下。

## 依赖

反编译需要涉及的工具有: apkTool.jar（单独下载[链接](https://ibotpeaches.github.io/Apktool/)）
重打包需要涉及的工具有: 签名文件（自备签名），jarsigner（JDK 集成），zipalign（Android SDK 集成）

## 反编译

只要准备好了 apktool 了之后，反编译其实就是一条命令行的问题就好了，看这里

```bash
java -jar apktool.jar d -f -s {apk_name} -o {output_folder}
```

## 重打包

重新打包需要自行准备好签名文件，那么需要经过这几个流程：
1. 重打包
2. 重签名
3. 对齐 APK

### 重打包

```bash
java -jar apktool.jar b {org_apk_folder} -o {output_apk_file}
```

### 重签名

```bash
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore {keystore} -storepass {storepass} -signedjar {signed_unalignedjar} {unsign_apk} {alianame}
```

### 对齐

```bash
zipalign -v 4 {signed_unalignedjar} {signed_alignedjar}
```

## 总结

为了方便我后续继续使用，我写了两个脚本，方便自己后续可以继续使用，详细可以到 [这里查看](https://github.com/Maxize/decompile_repack_apk) ，惊喜不断哦。

