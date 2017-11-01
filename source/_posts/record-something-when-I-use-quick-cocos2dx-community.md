---
title: 记录我在使用 Quick-Cocos2dx-Community 的过程
date: 2017-11-01 23:23:22
categories: cocos2dx
description: 每个人在入门一种新技能的的时候都会遇上一些坑，而这些坑也只有踩过才能更清楚的理解，从而避免再次踩进去，这一篇 Blog 我将记录一下关于我使用 Quick-Cocos2dx-Community 版本的问题。如果你也想了解一下，可以看看我的是否你也经历过。
tags: [Quick-Cocos2dx-Community, 使用过程, 思考]
---

## 背景

每个人在入门一种新技能的的时候都会遇上一些坑，而这些坑也只有踩过才能更清楚的理解，从而避免再次踩进去，这一篇 Blog 我将记录一下关于我使用 Quick-Cocos2dx-Community 版本的问题。如果你也想了解一下，可以看看我的是否你也经历过。

## 问题

* 一直以为 Cocos2dx 的性能会很好，其实没有数据支持，都是假象？

cocos2dx 从 3.0 开始支持自动批量渲染，不过也是没能改变在需要渲染的时候的开销，笔者自己测试的时候发现在低端机上的瓶颈还是很容易体现出来的。

```shell
测试设备： I9300(2012-5-3)
测试 demo： quick/samples/benchmark(3.7 以下有提供)
测试结果： 800 个
```

所以在使用的过程还是要关注性能的消耗，不要以为可以肆无忌惮的消耗，这个结果相对于我之前使用的引擎来说，半斤八两。

* 版本的改变，你是否已经准备好了？

3.7 是一个比较大的改变，使用之前的版本的项目估计只能保持在 3.6.5 了，而相对于新项目，还是比较推荐 3.7+ 进行开发的，因为废弃了很多代码，但是也让引擎更加轻便。

我印象深刻的是在使用框架提供的创建文本的方法时，不同的方法耗时不一样，这个不细抠你真的不会发现。这是我简单写的测试代码，有兴趣的可以测试一下，记得设备不要太高级哦，高级的设备很容易忽略了很多问题。

```lua
    local socket = require("socket")
    local beginTime = socket.gettime()
    local label1 = display.newTTFLabel({
        text = "Hello, World 1",
        font = display.DEFAULT_TTF_FONT,
        size = 28
    }):pos(display.cx, display.cy + 100)
    :addTo(self)
    print("create label1 cost time = ", socket.gettime() - beginTime)
    beginTime = socket.gettime()

    local label2 = cc.ui.UILabel.new({
        UILabelType = 2,
        text = "Hello, world 2",
        size = 28
    }):pos(display.cx, display.cy)
    :addTo(self)

    print("create label2 cost time = ", socket.gettime() - beginTime)
    beginTime = socket.gettime()
    local label3 = cc.Label:createWithSystemFont("Hello, world 3",
        display.DEFAULT_TTF_FONT,
        28)
    :pos(display.cx, display.cy - 100)
    :addTo(self)
    print("create label3 cost time = ", socket.gettime() - beginTime)

```

```bash
-- MacOS 上的结果
create label1 cost time =   0.00021910667419434
create label2 cost time =   0.00051999092102051
create label3 cost time =   7.5101852416992e-05
```

而我去探寻这个问题的时候就发现，quick framework 在为了提供一些更友好的借口的时候做了一些没必要的事，在 UILabel 的 ctor 中，调用了 `makeUIControl_(self)` 这个方法会耗时多 1ms. 虽然你看来 1ms 不多，但是作为一个使用频率特别高的基础 UI，特别是在列表中使用的时候，会在一帧创建多几个文本，就会造成卡帧了。所谓细数惊长计，能省一点是一点。

* 加密是否有用呢？

看这一篇别人总结的经验，为了更好的保护自己的代码，你可以想想其他的加密方式，当然这个东西防君子而用，也是做一下初级过滤，如果你也有更好的加密方式，欢迎交流讨论，毕竟不想自己辛辛苦苦写的劳动成果别人一下子就借用了。

[形同虚设的 cocos 默认加密](http://blog.shuax.com/archives/decryptcocos.html)

* 使用 GAF 转化 SWF 使用时，设计使用不同的格式，性能的差距是非常大的？

通过对同个效果进行多维度测试发现，在使用图形元件的时候可以更大程度的保证 cocos2dx 可以使用自动批量渲染的形式减少 drawcall，从而提高效率。

## 总结

每个完美使用过程都是经过细心调教的，而一开始你可能不会关注一些的问题，其实往往是因为没有什么契机需要去接触到，而当你慢慢品尝的时候，才会使你对自己所用的工具，所了解的引擎进行深一步的剖析，从而达到自己理想的状态。

多做测试，多做尝试，多做思考，就是我目前在使用 Quick-Cocos-Community 的状态。


