---
title: Sublime Text3 的自我修养
date: 2017-07-25 17:17:11
categories: Skill
description: 用过 sublime text3 都说好，你觉得呢？
tags: [sublime text3, skill]
---

## 介绍

## 插件安装与使用

## 我的插件与使用方法

## 命令行使用

笔者在 mac 下使用，有一天想着从命令行直接打开 sublime，以前都是 `open ./` 然后在 finer 下把文件／文件夹拖到 sublime 上，后面发现，多走了一步，能不能减少呢？

可以的，于是 Google 一圈，比较靠谱的解决方案是：

官方的介绍是这个： [osx_command_line](https://www.sublimetext.com/docs/3/osx_command_line.html)

通过上面有两个问题：
1. bin 找不到
2. 默认终端可以打开，但 zsh 打不开

第一个很好解决， `mkdir ~/bin` ，第二个就比较麻烦了，需要在 zsh 的配置文件 `~/.zshrc` 下加入相关配置才可以，汇总一下，需要操作的就是：

```bash
mkdir ~/bin
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" ~/bin/subl
export EDITOR='subl -w'

vi ~/.zshrc
# 在行末加入这几行
alias subl="'/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'"
alias nano="subl"
export EDITOR="subl"
source ~/.zshrc
# 重新打开 iTerm 就可以了
```

这样你就能愉快的在终端打开 sublime 了，四不四很开心，相关操作看这里：

```bash
$ subl --help
Usage: subl [arguments] [files]         编辑指定的文件 edit the given files
   or: subl [arguments] [directories]   打开指定的目录
   or: subl [arguments] -               编辑 stdin

Arguments:
  --project <project>: 载入指定的 project
  --command <command>: 运行指定的命令
  -n or --new-window:  打开一个新的窗口
  -a or --add:         添加文件夹到当前窗口
  -w or --wait:        返回前等待文件关闭
  -b or --background:  不激活该应用程序
  -s or --stay:        文件关闭后保持应用程序激活状态
  -h or --help:        显示帮助并退出
  -v or --version:     显示版本信息并退出

如果从标准输入 --wait 是隐式的。 使用 --stay 当文件关闭是不切换到后台控制台(只与是否有等待的文件有关)。

文件名可以通过加 :line 或者 :line:column 后缀来指定打开的定位。
```

## 参考

1. [如何在 mac 中用命令行时用 sublime 打开文件](https://segmentfault.com/q/1010000002397241)
2. [Setup Terminal for Sublime Shorcut "subl"](https://gist.github.com/barnes7td/3804534)
