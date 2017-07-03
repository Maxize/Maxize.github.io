---
title: 接入 Google 支付的问题汇总
date: 2017-06-14 14:13:57
categories: SDK
description: Google Play 之支付问题
tags: [pay, checkout, google play]
---

* 需要验证身份。您需要登录自己的Google账号
    - 后端的计费点跟后台的计费点不一致
    - package_name + ID (recommend)
* 无法购买您要买的商品
    - VersionCode 上传的包跟本地保持一致的 
    - Payment List 商品 ID 后端跟 Google 后台保持一致
    - Payment Key Google 后台的 Public_Key 保持一致
    - Test User 需要加入成为测试账号
        + 此处添加 https://play.google.com/apps/testing/package_name
    - Google Play 商店可用
    - Package Name 保持跟后台一致
* 调用 consumePurchase 出现 BILLING_RESPONSE_RESULT_ERROR
    - 在网上找了一圈，我认为比较靠谱的解答是**本地的缓存跟 Google 的不同步导致请求的时候出现超时**
    - 出现这个的时候试了很多种方法都无解，最后重新打了一个 release 包上传，隔了一晚上就正常了

