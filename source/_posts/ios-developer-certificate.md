---
title: iOS 开发者证书总结 in-house
date: 2017-05-27 00:39:11
categories: iOS
description: 我理解的 iOS 开发者证书，告别懵逼！
tags: [iOS, 开发者证书, in-hourse]
---

## 更新日志

* 2017-07-04: 新增获取证书的教程
* 2017-12-20: 新增 pem 证书说明

## 证书类型

### iOS 证书分两种类型

* 第一种为 $99 美元的，这种账号有个人和公司的区别，公司账号能创建多个子账号，但个人的不能。这种账号可以用来上传 app store
* 第二种为 $299 美元的，这种账号只能用于企业内部使用，不能用来上传 app store .也就是常说的 in-house 证书（用这种证书打出来的包能在任何 iOS 设备上运行，不需要苹果的验证、签名）-- 不要误解了这种账号即能上传 app store 又带有 in-house 的功能。**这是两种不同的账号**

### 开发 iOS 所需要的证书

* .certSigningRequest
    - 证书请求文件，用于生成证书文件。一般由开发者通过 Mac 机使用 KeyChain 打开「钥匙串访问」 -> 「证书助理」 -> 「从证书颁发机构请求证书」生成并保存在本地磁盘，以后生成的证书都使用它，唯一不可删除，删除了重新申请的就不一样了。
* .cer 类型的证书
    + 这种证书创建的时候会让你选择一个本地的私有证书上传。也是开发所必须的证书，可以根据选择绑定相应的 app IDs 
* .p12 类似的证书
    + 如果一个开发者账号有多个人用，那么这个证书就要从本地 Keychain Access 里把上面的 .cer 证书导出来。然后选择后缀名为 p12。
* .mobileprovision 文件
    + 创建的时候它会把开发者账号里所有的设备加进去。只有 xcode 安装了这个文件才能调试（注意。这里的 mobileprovision 如果是 distribution 那么它是不能在真机上测试的，只有上传到 app store 通过了 apple 的审核才能正常安装到 iOS 设备里面）
一般来讲只要有了上面的这三种证书文件，就可以在真机上调试了。（注意 developer 类似的证书只能用于测试，distribution 证书只能用来上传 app store 没上线之前不能安装到 iOS 设备）

* .pem 类型证书
    + 在使用推送证书的时候需要生成 「pem」证书给服务器，方便服务器给苹果推送服务器推送消息。生成的需要我们导出的 「p12」 证书，分别是 「key」 和 「cer」，相关的操作命令如下：

```shell
    openssl pkcs12 -clcerts -nokeys -out aps_development_cer.pem -in aps_development_cer.p12
    openssl pkcs12 -nocerts -out aps_development_key.pem -in aps_development_key.p12
    openssl rsa -in aps_development_key.pem -out aps_development_key.pem
    cat aps_development_cer.pem aps_development_key.pem > aps_development.pem

    # test connect
    # openssl s_client -connect gateway.sandbox.push.apple.com:2195 -cert aps_development_cer.pem -key aps_development_key.pem

```

## 如何获得对应的证书

关于不同证书的申请途径，参考： 
1. [iOS证书及描述文件制作流程](http://docs.apicloud.com/Dev-Guide/iOS-License-Application-Guidance#0)
2. [iOS开发证书与配置文件的使用](http://www.jianshu.com/p/9d9e3699515e)

