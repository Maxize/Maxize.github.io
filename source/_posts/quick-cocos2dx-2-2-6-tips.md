---
title: quick-cocos2dx-2.2.6-tips
date: 2017-06-13 11:23:59
categories: cocos2dx
description: 使用 quick-coco2dx 问题记录
tags:  quick2.2.6
---

## quick2.2.6 问题记录

1. luasocket 不能使用方式 用下面地址的文件替换文件重新编译
    替换 **lib/cocos2dx/scripting/lua/lua_extensions/socket** 下的 [socket_scripts.c(https://github.com/chukong/quick-cocos2d-x/blob/master/lib/cocos2d-x/scripting/lua/lua_extensions/socket/socket_scripts.c)

2. 预编译的包 framework_precompiled.zip 有问题需要自己编译
    编译方式 进入到 framework 所在目录 
    ```shell
    compile_scripts -i framework -o framework_precompiled.zip -p framework
    ```

3. quick 文件需要做好上面两步骤后**重新编译** (笔者 xcode 8.3.3 编译的时候因为 SDK 更新了，所以无法编译)

4. cocos2dx-3.X 以后的版本使用了 c++11 新特性 不支持 xp 如果要支持 xp 平台最后的版本为 cocos2dx 2.2.6
