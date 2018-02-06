---
title: 利用 sigh 重签名 IPA
date: 2018-02-06 13:02:09
categories: iOS
description: 偶然在 1024 技术讨论区学的此技能，不私藏，来献丑一下
tags: [iOS, resign, IPA]
---

## 安装 Homebrew

在终端先后执行下面2命令行安装，等待进度完毕

``` bash
xcode-select --install 
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 安装 ruby

在终端执行下面命令安装 ruby，等待进度完毕（输完密码可能在较短时间无反应）

``` bash
brew install ruby 
```

## 安装 sigh 脚本

执行下面安装命令

``` bash
sudo gem install sigh 
```

若出现以下报错

``` bash
ERROR: While executing gem ... (Errno::EPERM) 

Operation not permitted - /usr/bin/rougify 
```

则安装命令修改为

``` bash
sudo gem install -n /usr/local/bin sigh 
```

## 使用 sigh 脚本开始重新签名

1. 在终端输入 sigh resign，回车
2. 把要签名的 ipa 文件（路径、包名不要有中文，例如可以把小草改为 1.ipa）拖到窗口上，回车
3. 填写用来签名的证书名（钥匙串中的完整名字），回车
4. 把项目的配置文件 .mobileprovision 文件拖到窗口上，回车
5. 好了，resign 脚本会自动更改 bundel id，签名并重新打包。

完成后提示 Successfully signed ，新生成的包会替换原有文件

## 进行安装

由于 iTunes 在 12.7 取消安装应用，所以本人使用 iMazing 安装的，也挺方便的。

## 引用

1. [在未越狱IOS设备上利用自带“终端”工具重签名安装草榴社区官方客户端](http://t66y.com/htm_data/7/1709/2619980.html)
2. [MacOS 使用“终端”对 ipa 重签名](http://www.zhangbingliang.com/3788.html)
3. [Homebrew 官网](http://brew.sh/index_zh-cn.html)
4. [Sigh 脚本 github 地址](https://github.com/fastlane/fastlane/tree/master/sigh#resign)
5. [最简单的重签名应用的方法](http://iosre.com/t/topic/2966)
6. [iOS 的 ipa 重签名](http://www.jianshu.com/p/3f57d51f770a)
7. [Cannot install cocoa pods after uninstalling, results in error](http://stackoverflow.com/questions/30812777/cannot-install-cocoa-pods-after-uninstalling-results-in-error/30851030#30851030)
8. [mac 用终端对 ipa 包重新签名 - 胡东东博客](http://www.hudongdong.com/skill/363.html)
9. [Mac OS下包管理器 Homebrew 的安装与使用](http://www.jianshu.com/p/d229ac7fe77d)
