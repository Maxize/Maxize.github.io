---
title: eclipse 如何使用 aar
date: 2018-10-17 16:49:00
categories:android
description: 在 Android Studio 作为 Android 的默认 IDE 之后，eclipse 慢慢淡出视野，但还有不少公司还是用着 eclipse，那么现在很多开源项目包括第三方 SDK 都慢慢抛弃输出 jar，使得 eclipse 能支持的越来越少。
tags:[eclipse, aar, android]
---

## 什么是 AAR ？

AAR 是 Google 专门为 Android Studio 推出的一种库文件格式，用于便捷的分享和使用 Android Library 项目，不兼容 Eclipse。

## 哪里可以下载 AAR ？

[MvnRepository](https://mvnrepository.com/) 是目前 Android 下第三方库的仓库聚合，所以，你所有使用 Android Studio 引入的可以在这里搜索并下载。

## 如何把 AAR 变成类库？

* 把 `.aar` 改为 `.zip` 并解压
* 在解压的目录下
    * 新建 `libs` 文件夹，把 `classes.jar` 和 `jni` 文件夹下的所有文件移到 `libs` 目录
    * 新建 `project.properties`，并加入如下内容

    ``` bash
    # 如果有混淆的话把这一行打开
    #proguard.config=${sdk.dir}/tools/proguard/proguard-android.txt:proguard-project.txt

    # Project target.
    # 当前指定的 android 版本
    target=android-27
    android.library=true
    # 是否有依赖，有的话自行添加
    # android.library.reference.1=../google-play-services-basement
    ```

    * 删除 `aapt` 和 `jni` 文件夹以及 `R.txt`
* 通过 eclipse 导入库项目即可

到此，你就能愉快的把 aar 改造成 eclipse 可用的库。

## 参考

* [dandar3](https://github.com/dandar3) 整合了 google 提供的很多相关的库

