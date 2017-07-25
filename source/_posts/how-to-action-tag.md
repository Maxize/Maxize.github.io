---
title: Git tag 操作
date: 2017-07-25 10:50:23
categories: Git
description: 只会添加 tag 是不对的，我们能删能改。
tags: [Git, tag]
---

## 背景

原谅我对 tag 的操作还是很陌生，之前打的 tag 也是各种乱七八糟，为了详细的了解一下 tag 操作，决定用一篇博客来详细阐述一下。

比较系统的介绍看这里： [2.6 Git 基础 - 打标签](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE)

## 操作

在 sourceTree 上操作的时候，发现明明我已经删除 tag 了，但是还是没有正确的删除 tag，这就让我很纳闷了。之前因为懒，没有认真对待，但是后面发现，项目跑的久了，不系统的梳理一下标签会很混乱。

图形操作不行，咱就用命令行呗。

### 查看标签

我们在`标签`那一栏看到的其实是通过这个命令来获取的：

```bash
git tag
```

当然可以使用这个来过滤一下，如果 tag 较多的话，过滤前缀为 `v1.0.` 的 tag

```bash
git tag -l 'v1.0.*'
```

### 删除标签

删除本地 tag:

```bash
git tag -d v1.0
```

只删除本地的没有用，下次 fetch 的时候还会拉下来，再删除远程仓库中的 tag ，跟删除远程分支的语法类似

```bash
git push origin :refs/tags/v1.0
```

### 添加标签

Git 使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。

一般我们都建议使用含附注型的标签，有自身的校验和信息，包含着标签的名字，电子邮件地址和日期，以及标签说明，存储的一个独立对象，两者的操作如下：

* 含附注的 tag

```bash
# -a 取 annotated 的首字母
# -m 标签说明
git tag -a v1.0 -m 'my version 1.0'
```


* 轻量级的 tag

```bash
git tag v1.0
```

**查看标签**

```bash
git show v1.0
```


### 补充标签

有时候我们发布版本的时候，然后忘记给添加标签，肯定也是可以继续补上的，提交的时候记得附上文件的**校验和**即可，查看**校验和**使用 `git log` ，所以对应的命令应该是：

```bash
# 9fceb02 为你某次提交的校验和
git tag -a v0.8 9fceb02 -m 'my version 0.8'
```

### 签署标签和验证标签

这两个个人暂时不会用到，所以就不讲了，可以从这里出门查看： [签署标签](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE#签署标签) ， [验证标签](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE#验证标签)


### 推送标签

所有的操作如果没有使用 push 那么都是在本地修改而已，可以使用 `git push origin [tagname]` 进行推送，如果一次操作的 tag 较多，可以使用 `git push origin --tags` 推送多个。

## 最后

写完这个之后，我也受益良多，以前一直觉得标签貌似就是一个标记而已，但实际操作起来还是有不少要注意的，看完希望可以帮到你！
