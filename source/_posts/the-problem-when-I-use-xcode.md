---
title: 使用 Xcode 遇到的问题集锦
date: 2017-11-08 11:30:31
categories: iOS
description: 讲良心，Xcode 这款免费的开发工具是开发者的利器，完好她可以让你的开发如虎添翼，而我也只是摸石头过河的一点点掌握，不同于以往使用 Android Studio ，现在我会慢慢记录一些过程中的问题，期待哪一天回头看的时候让自己感动。
tags: [Xcode, iOS]
---
## 背景

不知你是否也跟我一样，在第一次开始使用 Xcode 的时候遇到各种各样的问题，反正我是遇到了。
于是，写这篇博客收录一些在过程中遇到的问题，并尝试提供解决方案，如果有一些你也遇到了，希望能给你带来帮助。

### 更新日志

* 2017-11-08 init this news
* 2017-12-20 
    - Xcode 的帮助文档
    - 更新系统与更新 Xcode（ Xcode 9.2 升级带来的问题）
    - 打包选择 Generic iOS * Device ?

## 问题

### Xcode 的帮助文档

本身 Xcode 提供的官方文档是最齐全，最权威的，理论上你可以在这里找到任何你想知道的答案，虽然有一些坑是没有明说的，你知道的，但也不影响它成为你日常使用中来查询一些问题的好去处。

可惜没有中文文档，不过你可以使用 Chrome 浏览器翻译一下，勉强还是可以看的。

传送门：[Xcode Help](http://help.apple.com/xcode/mac/current/#/)

Xcode 中打开，在菜单栏 Help -> Xcode Help 下打开

### Xcode 兼容问题

一个好的 iOS 开发理论上会安装多个 Xcode 来解决一些兼容性问题，包括设备升级了系统，但 Xcode 依然是旧版本的问题。

那么电脑中安装多个 Xcode 是可以解决这一类问题的。

* 安装多个 Xcode

1. 最新版本的 Xcode 通过 App Store 下载更新

2. 官方下载地址 [Xcode](https://developer.apple.com/download/more/?=for%20Xcode) 下载旧版本的 Xcode ，更改 Xcode 名字，把 Xcode_x.app 拖到应用程序即可。

除了安装多个 Xcode 还需要加入 iOS 的 SDK 版本。

* 安装 iOS SDK

找到 Xcode SDK 的路径： `/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/`

获取 iOS SDK：

一般 Xcode 都会带有默认支持的 iOS SDK，升级 Xcode 也会把最新的加到包里面，但是对于一个稳定版本，就需要自己从其他版本的 Xcode 中拷贝过来了。

目前我收集到的你可以通过这里去下载： [百度云](https://pan.baidu.com/s/1hsw54SO)【不定期更新】

### Development cannot be enabled while your device is locked.

出现这种问题，解决方案：点击 设置 --> 通用 --> 重置  --> 重置位置和隐私

原因： 手机连接 Mac 时，没有点击授权信任，重置一下，连上手机等 Xcode 把相关东西 copy 到设备就可以调试了。

### 打开 Organizer 卡

解决：

打开 Finder， command + shift + G 跳转到 `~/library/Developer/Xcode/products` 删除你「看不顺眼的」，速度提高杠杠的，推背感强。

因为在使用开发者账号登录的时候，为了方便管理，Xcode 帮你智能的缓存了列表，如果 APP 发布多了，打开速度就自然下去了，有时候你可以来一次解放。

### 更新系统与更新 Xcode

本来觉得这个问题没必要提起的，但是最近在使用过程中还是吃了一些亏，幸好自己还是安装了多个 Xcode 版本才避免了悲剧。

事情发生在 12 月 5 日，那一天我刚更新了 Xcode 9.2 Release 版本，然后刚好晚上需要发包了，结果之前可以用的 Xcode 9.0 报了这样一个错误「ERROR ITMS-90534」图片可从下面链接查看，瞬间懵逼。系统是最新的 10.13.1（后悔升级，电脑变卡了，闪退多了），Xcode 9.2 ，你在欺负我的智商么？

在网上搜了一下关于这个问题，不止出现在这个版本，8.0 升级 8.3 也出现了，从这里我就明白了，追求新版本是有代价的，升级这种事情要慎重。
问题链接从这里进， [Xcode 9.2 Upload to App Store fails with description length and invalid toolchain errors](https://stackoverflow.com/questions/47644270/xcode-9-2-upload-to-app-store-fails-with-description-length-and-invalid-toolchai) 除了 [Stack overflow](https://stackoverflow.com) 上有反馈，[Apple Developer Forums](https://forums.developer.apple.com)也是立马就有人跳出来。

目前保持不变，**时隔小半个月，已经可以使用 Xcode 9.2 发布应用了**，代码配置什么都没有更改，苹果爸爸，有 Bug 能通知一下么？这么随便，这么任性合适么？

### 打包选择 Generic iOS * Device ?

我觉得这个问题很蠢，但是你真的知道其中缘由么？

好吧，我承认是因为我在打包的时候忘记拔掉手机，手快点了通过设备打包了。

然后各种担心，会不会跟直接使用 Generic iOS * Device 的不一样呢？会不会被拒呢？等等。

面对未知的东西，人就容易恐惧，所以我也不例外，本着求知的欲望，我还是要挖掘一下这个问题。

最后我在 Xcode 的官方帮助文档里面找到了答案，当然最根本的问题还是没有解决，但有一个权威的解答也是可以接受的。

```bash
For iOS, tvOS, and watchOS apps, choose a generic device—Generic iOS Device, Generic tvOS Device, or Generic iOS Device + watchOS Device—or choose your device name from the Scheme toolbar menu. If a device is connected to your Mac, the device name appears in the Scheme toolbar menu. When you disconnect the device, the menu item changes to the generic device name.
```

表现上，两者是没有差别的。

参考链接： [Create an archive of your app](http://help.apple.com/xcode/mac/current/#/devf37a1db04)

一般打包推荐的步骤：

1. 在 Xcode 下修改 **toolbar** 上的 target 指向为 `Build Only Devices -> Generic iOS * Device`
2. 去掉所有的断点，可以在 Xcode 中选择 Show the Breakpoint navigator 查看是否有断点保留着

那么问题来了，什么时候需要选择不同的 target 来打包呢？

在输出静态库的时候需要指定不同的 target 并合并，这是考虑兼容性问题，不提供的平台的话，就会出现在要么是真机跑起来找不到，要么就是模拟器找不到。

```shell
lipo -create 第一个.a文件的绝对路径 第二个.a文件的绝对路径 -output 最终的.a文件路径。
```

如何使用 Xcode 编译静态库，可以参考这个：[Xcode 创建.a和framework静态库](http://www.jianshu.com/p/43d55ae49f59)

### 新版本 Xcode 下在旧系统下运行报 Framework 异常解决

``` bash
dyld: Library not loaded: /System/Library/Frameworks/UserNotifications.framework/UserNotifications Referenced from: /var/containers/Bundle/Application/245FDD8F-8BA5-46DF-96A4-EF6C84BA3361/dummy.app/dummy Reason: image not found
```

哪个 Framework 有问题，在 Link Binary With Libraries 中从 Require 改为 Optional 。

## 总结

有些问题一定要自己经历过才知道，而庆幸自己遇到的问题都有前人踩过坑，以后遇上了未被开发的 Bug 再来总结吧，站在巨人的肩膀上就是舒服点。


