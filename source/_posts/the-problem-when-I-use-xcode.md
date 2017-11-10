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

## 问题

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



## 总结

有些问题一定要自己经历过才知道，而庆幸自己遇到的问题都有前人踩过坑，以后遇上了未被开发的 Bug 再来总结吧，站在巨人的肩膀上就是舒服点。


