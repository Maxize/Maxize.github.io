---
title: 关于 Sublime 的 Tips
date: 2016-09-30 01:07:44
categories: Usage Tips
description: 巧用 sublime 正则表达式，轻松替换文本
tags: sublime
---
### 使用正则表达式
* 历史遗留问题，现在看到关于这种样式的lua代码暗不爽
```Lua
    class.func = function ( self )
```
* 想把他们替换成
```Lua
    function class:func ()
```
* 比较懒，一个个替换是在太麻烦了，幸好sublime有正则匹配，好吧。
* 开启正则匹配，**ctrl+H** 调出替换 面板，点击 左下角 **'.*'**
* 所以正确的姿势是：
```Lua
    -- 用这个匹配
    class.(.*) = function \( self
    -- 用这个替换
    function class:$1 ()
```
* 效果不错，不错为什么会有一些逗号在左边括号后面呢？
* 好吧，再来：
```Lua
    -- 用这个匹配
    class.(.*) = function \( self,
    -- 用这个替换
    function class:$1 ()
```
* 嗯，替换效果还不错。
* 不过这样要匹配两次，作为程序员，不允许呀。得想想办法
```Lua
    (.*)\.(.*) = function(\s?)\((\s?)self(,?\s?)(.*)(\s?)\)
    function $1:$2 ($6)
```
* 相关说明：
    - \s 匹配任意空格符
    - .* 匹配任意字符
    - ?  匹配0或1个
* 完美！ 可以睡觉了。
