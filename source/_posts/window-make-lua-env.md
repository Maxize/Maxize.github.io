---
title: Windows 下搭建 Lua 环境
date: 2017-05-27 00:53:14
categories: Lua
description: Lua 环境的搭配，你懂了么？
tags: Lua
---

1. 首先下载 [lua](https://www.lua.org/download.html)，解压到任意位置（路径中不要有中文）

2. 打开 VS 的命令行工具，**VS2013 x64 本机工具命令提示**。输入：__cd /d C:\Lua_program\Lua\lua-5.1.5\src__ **C:\Lua_program\Lua\lua-5.1.5\src**替换为你的 lua 解压路径的 src 目录

3. 将 Lua 解压路径下的 etc 目录里的 luavs.bat 拷贝到 src 目录下

4. 执行 bat 文件，即在第 2 步打开的命令行中输入：luavs.bat

5. 经过批处理，生成 luac.exe, lua.exe, lua51.dll, lua51.exp, lua51.lib, 共 5 个文件，按文件夹的修改时间倒序，即可在最前排显示

6. 在 src 同级目录下，创建 bin 目录，copy 上面的 5 个文件到 bin 目录下

7. 配置 Lua 的环境变量
    右击我的电脑，属性，环境变量， Path ，编辑，在最后一行添加你刚才创建的 bin 路径到 Path 里，记得先加个分号，在粘贴你的路径，最后再加个分号

8. 测试
    Commond + R，输入 cmd ，输入：lua -version 。即可看到 Lua 环境配置成功。

