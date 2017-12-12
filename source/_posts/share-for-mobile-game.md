---
title: 游戏分享的问题
date: 2017-12-01 18:36:24
categories: 业务功能
description: 开发游戏，分享功能是一个不得不面对的功能，而且不同的平台的要求和机制也不一样，这里开一篇来记录一下自己遇到的一些问题，方便自己查阅。
tags: [Facebook, Share, Games]
---

## 背景

本来不想记录的，让自己想铁下心来写这篇 Blog 的原因是前阵子在接入 FB 分享的时候，明明已经处理好了，可是出了问题，想去找一下 FB 的分享调试器的时候，居然绕了一大圈才找到，这样才萌生了念头。

废话不多说，本 Blog 我想分为以下几方面去介绍。

1. 一些常见平台分享功能的限制和要求
2. 我接入 SDK 的惯用伎俩
3. 我接入过的一些平台踩过的坑

## 正文

### 常见平台分享的要求

我们都知道不同平台，分享功能的要求是不一样的，每个平台都会有一些限制，个中要求只有接过了才能明白，所以如果有人列一个大纲，帮你介绍一下这个东西的话，那是不是就会很爽很开心。

目前已经有不少提供第三方 SDK 帮我们聚合分享功能了，但有时候我们还是需要自己操刀接入一些，有这么一个东西还是好处大大的。

之前浏览网页的时候发现了这么一个网页，后面也没有记录下来，然后花了点时间把他找回来，特此奉献一下。

[iOS不同平台分享内容的详细说明](http://wiki.mob.com/ios%E4%B8%8D%E5%90%8C%E5%B9%B3%E5%8F%B0%E5%88%86%E4%BA%AB%E5%86%85%E5%AE%B9%E7%9A%84%E8%AF%A6%E7%BB%86%E8%AF%B4%E6%98%8E/)

其实我害怕哪一天这个网址会失效，有想法想把这篇自己再重新用 markdown 写一下，留下来备份。

由于内容过多，有兴趣的可以自己翻阅这个地址查看，我就不在这里展示了。

### SDK 你是怎么接的

1. 查看官方文档和官方 Demo(最喜欢有 GitHub 的 SDK)
2. 遇到问题了，确认文档已经都看好了
3. 有技术支持，可以直接咨询，不然只能搜索引擎走起
4. 项目中接入的形式
    * 使用 AnySDK or 其他聚合 SDK
    * 如果有 C++ 版可以优先考虑接
    * 直接原生接入（推荐，省心，跟原生一样处理就好了）

### 我踩的坑

楼主已经接过的 SDK 有：

* 国内的 SDK：

1. 新浪微博
2. 微信（登录与支付）
3. 短代
4. 银联支付
5. 友盟
6. ......

* 国外的 SDK：

1. Facebook
2. BluePay
3. Mol 支付
4. E2Pay
5. TalkingData
6. fortumo 支付
7. ......

其他想不起来了，哈哈。

貌似扯的有点远了，要回到分享功能上了。

---

楼主最近在处理 FB 分享的时候遇到的问题以及接入 FB 可以使用的工具

* Facebook SDK

1. 官方提供的工具

官方有一个网址，提供了我们在接入 SDK 可以使用的工具，看这里[工具和支持](https://developers.facebook.com/tools-and-support/)

主要的工具有：

* [图谱 API 探索工具](https://developers.facebook.com/tools/explorer/) 
* [分享调试器](https://developers.facebook.com/tools/debug/sharing)

「图谱 API 探索工具」是我之前在客户端使用他来发送分享功能的，因为可以自定义内容，而且也可以方便测试，但他是需要获取玩家的 `publish_actions` 权限的，而且在调用的时候就要提前获取。

「分享调试器」是 Facebook 通过爬虫抓取你的分享地址来展示，可以图文，也可以是视频，但是对应的地址需要提供类似以下标签。

```html
<html xmlns="http://www.w3.org/1999/xhtml" 
xmlns:og="http://ogp.me/ns#" xmlns:fb="http://www.facebook.com/2008/fbml">
<head><title>The Rock (1996)</title> 
<meta property="og:title" content="The Rock"/> 
<meta property="og:type" content="movie"/> 
<meta property="og:url" content="http://www.imdb.com/title/tt0117500/"/> 
<meta property="og:image" content="http://ia.media-imdb.com/rock.jpg"/> 
<meta property="og:site_name" content="IMDb"/> 
<meta property="fb:admins" content="USER_ID"/> 
<meta property="og:description" content="A group of U.S. Marines, under command of a renegade general, take over Alcatraz and threaten San Francisco Bay with biological weapons."/>
 ... </head> ... 
</html>
```

官方给的一个指导建议：

[最佳实践](https://developers.facebook.com/docs/sharing/best-practices)

大家可以优先看一下这个指导。

2. 分享有哪些方式

从[最佳实践](https://developers.facebook.com/docs/sharing/best-practices)那里我们可以看到，目前支持的方式有：

* 分享对话框 （类似微信分享到朋友圈）
* 分享到 Message （类似微信分享给好友）
* 分享 "关注" 按钮 （跳转到粉丝页）
* 分享 "like" 按钮
* 通过分享 API 分享

因为 Facebook 有爬虫可以帮你抓取页面，所以你可以自定义你的分享链接地址，而分享的链接可以指定跳转到对应的游戏，这一点可以更容易的导量到你的应用，让分享的意义放大化。

总结就是，使用前三种分享，是一个免费的带量行为，值得我们花时间去完善它。

说到这里，其实就是可以通过 FB 接口获取好友的信息，然后邀请好友玩游戏，如果有 Web 版本的游戏体验会更好，不然就是跳转到应用商店或者启动应用。

3. 有一些官方隐藏的坑

FB 比较坑爹的地方是 API 会随着版本发布经常变动，而且时不时还会抽风。

笔者遇到的一个情况就是，之前明明分享出去的地址是可以抓取到图片的，然后突然之间就不可以了，表现就是分享时设备也不显示了，分享出去之后也没有。当然这个是有一定要求的，比如说图片的尺寸要求（1200 * 640 以上），分享的链接需要指定参数。

后面没有改动，分享出去的时候能看到图片了，但是分享时还是看不到图片。

相关的要求可以参考这个（抓取不到图片）：

* 图片尺寸
    - 0 x 0  ~ 199 x 199 =  会抓不到图
    - 200 x 200 ~ 599 x 314 = 会抓到小图
    - 600 x 315 ~ 1500 x 1500 = 会抓到大图
    - 1501 x 1501 ~  以上 = 会抓到小图
* 图片大小不可超过 5MB
* 使用 og:image:width 跟 og:image:height  来直接指定图片宽高

参考：[Facebook 文章分享的預覽圖片，顯示為寬版大圖的設定方式 - og:image 的 image size 條件](http://sweeteason.pixnet.net/blog/post/42228946-facebook-文章分享的預覽圖片，顯示為寬版大圖) 查看更多

当然还有两个点是血的教训，也要提一下。

1. 对同个好友不要邀请过多次
2. 一天不要发送太多的帖子
3. 登录账号 IP 不要经常变动

这几点都会导致 FB 封账号，再严重一点 FB 会觉得你的应用分享过于频繁，会把你的应用也封了，而申诉都非常麻烦，尽量不要尝试。

## 总结

这篇博文断断续续写了一两周，感觉还是没有把都要说的说明白，后续看看再来补充好了。

如果对你有帮助，那么就最好不过了。


