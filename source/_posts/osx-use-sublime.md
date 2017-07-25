---
title: Mac OSX 下的 Sublime Text3 与 命令行
date: 2017-07-25 18:02:54
categories: Skill
description: 有没有想过哪一天在命令行下也能愉快的使用 Sublime Text3 么？
tags: [Sublime Text, skill]
---

## 背景

笔者在 mac 下使用，有一天想着从命令行直接打开 sublime，以前都是 `open ./` 然后在 finder 下把文件／文件夹拖到 sublime 上，后面发现，多走了一步，能不能减少呢？

## 解决

答案是可以的，于是 Google 一圈，整理了比较靠谱的解决方案。

先看看官方的介绍： [osx_command_line](https://www.sublimetext.com/docs/3/osx_command_line.html)

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

这样你就能愉快的在终端打开 Sublime Text 了，四不四很开心，相关操作看这里：

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

## Sublime Text 下打开命令行

能够从命令行下打开，那么也就能从 Sublime Text 下打开命令行咯。

没错，我也觉得是这样的，网上找了一圈，有个东西完美解决，这里也介绍一下。

### 安装插件 Termineral

`command + shift + p`，输入 `pip`，打开 `Package Control:Install Package`，等会弹框出来找到 `Termineral`， 点击安装

安装完成之后需要配置一下， 打开 `Sublime Text -> Preferences -> Package Setting -> Termineral -> Settings-User`

在下面加入配置：

```xml
{
    // The command to execute for the terminal, leave blank for the OS default
    // See https://github.com/wbond/sublime_terminal#examples for examples
    "terminal": "iTerm2-v3.sh"
}
```

这里我是用的是 `iTerm2`，如果是 `iTerm` 的话，`"terminal": "iTerm.sh"` ，如果是默认终端， `"terminal": "Terminal.sh"`。

### 如何使用

我们可以参考这里的快捷键配置： `Sublime Text -> Preferences -> Package Setting -> Termineral -> Key Bindings-Default`

```xml
[
    { "keys": ["super+shift+t"], "command": "open_terminal" },
    { "keys": ["super+shift+alt+t"], "command": "open_terminal_project_folder" }
]
```

这样你就能愉快的在 Sublime Text 下打开终端了，四不四很兴奋！

## 参考

1. [如何在 mac 中用命令行时用 sublime 打开文件](https://segmentfault.com/q/1010000002397241)
2. [Setup Terminal for Sublime Shorcut "subl"](https://gist.github.com/barnes7td/3804534)
3. [sublime_terminal](https://github.com/wbond/sublime_terminal#examples)
