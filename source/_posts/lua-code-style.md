---
title: 项目代码规范指南
date: 2017-07-05 17:45:15
categories: Docs
description: 每个项目都有一套规范，而且期望所有人都按照这个规范来遵循。
tags: [lua, code style]
---

## 格式

- 代码文件以 **`.lua`** 后缀结尾
- 代码文件保存为 **`utf-8 without bom`** 编码格式
- 代码缩进为 **4个空格**，编辑器使用tab缩进的，请修改为 *tab = 4\*blank*
- 每一行代码字符长度不超过 **半个屏幕**（以24寸屏为标准）。*翻译文件，自动生成文件除外*
- 代码 **注释** 的标准是 **少而精** ，主要针对`巧妙的`、`晦涩的`、`有趣的`、`重要的`地方加以注释。所提供的注释应该是解释代码 **为什么这么写** 以及 **这段代码的目的**
- 文件头信息
    
    ```lua
    -- XXX.lua  文件名
    -- Author: 作者
    -- Data:   创建时间
    -- Description:  作用描述
    ```
- 编写公共函数时（framework 目录下），提供 **函数描述信息**，便于以后使用 [LDoc](https://github.com/stevedonovan/ldoc) 进行文档生成，更多更详细的写法可以参考[ LDoc 的文档](http://stevedonovan.github.io/ldoc/manual/doc.md.html)
    
    ```lua
    ---  XXX 简单描述
    --  @param XX -- 参数
    --  @param XX
    --  @return   返回值
    
    
    -- 举例
    
    --- foo explodes text.
    -- It is a specialized splitting operation on a string.
    -- @param text the string
    -- @return a table of substrings
    function foo (text)
    ....
    end
    ```


## 命名
- 命名以 **简洁明了** 为准，具备描述性，尽可能避免缩写
- 命名选择优先 **贴近项目**，使用常见且通俗易懂的词汇术语，比如德州扑克相关术语
- 属性命名选`名词`，方法函数选`动词`，布尔逻辑加`is`
- 文件名，采用 **大驼峰式** ，即所有单词首字母均大写，保持和代码内的类名\模块名一致，如：`FileName`
- 普通变量，命名采用 **小驼峰式**，即首个单词首字母小写，其他单词首字母大写，如：`firstName`
- 成员变量，命名采用 **小驼峰式**，加前缀 **m_**，如 `m_firstName`
- 类静态变量，命名采用 **小驼峰式**，加前缀 **s_**，如 `s_firstName`
- 全局变量，采用 **全大写，加下划线**，如 `APP_ID`（尽可能少用全局变量），禁止 **下划线开头的全大写**，如`_GLOBAL`，该方式为 Lua 内部使用，容易冲突
- 普通函数，成员方法命名采用 **小驼峰式**
- 全局函数，采用 **全小写，加下划线**，如 `get_screen_width()`（配合当前引擎）
- 常见`i,j,k,v`尽可能只用于for循环内部，`_`可用于占位符，表示不做处理
    ```lua
    for i = 1, 10 then
    end

    for k, v in pairs(t) then
    end

    for i, v in ipairs(t) then
    end

    -- _ 标识不做处理，只用来占位，但本质上还是可以使用的变量
    for _, v in pairs(t) then  
    end
    ```
- UIEditor 控件命名规则
    ```lua
    Image            采用 imgXxx
    Button           采用 btnXxx
    Text             采用 txXxx
    RadioButton      采用 rbXxx
    RadioButtonGroup 采用 rbGroupXxx
    CheckBox         采用 cBoxXxx
    CheckBoxGroup    采用 cBoxGroupXxx
    TextView         采用 tvXxx
    EditText         采用 etTxXxx
    EditTextView     采用 etTvXxx
    View             采用 viewXxx
    ListView         采用 lvXxx
    ScrollerView     采用 scrViewXxx
    TableView        采用 tabViewXxx
    Slider           采用 sliderXx
    ```
```
任何地方命名禁止使用 arg，该名字在旧版 lua（<5.1) 中用于提供可变变量的支持（高版本 lua 在编译时可以决定是否兼容此特性）。
```

## Lua 书写
- 尽可能使用 `local` 修饰变量；存取更快，作用域外自动释放内存，不污染全局命名空间
- 使用 `or` 做默认值实现，范式：`param = param or defaultValue`

    ```lua
    function foo(value)
        value = value or {}
        ...
    end
    ```
- 单行注释用`--`，块注释用`--[[ --]]`
    ```lua
    -- 后面跟一个空格
    -- line comment

    --[[
        block comment
    --]]
    ```
- 字符串常用双引号`" "`，需要大量保留书写格式的文本可用`[[ ]]`
- 使用表的构造器模式来初始化表，相对减少部分 **重新哈希** 的消耗；（注意缩进规范）
    ```lua
    -- good
    local t = {
        a=1,
        b=1, 
        c=1
    }
    
    -- good
    local t = { 1,1,1,1,1 }

    -- bad
    local t ={}
    t.a = 1
    t.b = 1
    t.c = 1
    
    -- very bad
    local t = {}
    for i = 1, 5 then
        table.insert(t, 1)
    end
    
    ```
- 表中插入数据，除非特殊情况，一般 **少用** `table.insert`
    ```lua
    -- 常见情况不推荐使用，高层函数调用，性能受影响
    table.insert(t, value)

    -- 推荐一, 可读性与性能平衡
    t[#t+1] = value

    -- 推荐二，做大规模频繁（循环）插入时使用, 性能最好
    local index = 1
    for i = 1, 100000000 then
        t[index] = value
        index = index + 1 
    end

    ```
- 表中元素使用 **`,`** 分隔，不要用 **`;`**
    ```lua
    -- good
    local t = {
        a = 1,
        b = 1
    }

    -- bad
    local t = {
        a = 1;
        b = 1
    }
    ```
- 函数创建使用 `function name(...) ... end`的形式
    ```lua
    -- 全局函数
    function global_func( )

    end

    -- 成员方法
    function ClassName:func( )

    end

    -- 局部函数
    local function func( )

    end
    ```
- 函数调用使用 `:`，注意函数内使用的`self`是不是真正的 self（禁止为 self 重新赋值）
    ```lua
    ClassName:func()
    ```
- 简短的函数可以写在一行，若写成多行，注意缩进
    ```lua
    -- good
    function foo()  return true end

    -- good
    function foo()
        return true
    end

    -- bad
    function foo() return true 
    end

    -- bad
    function foo()
    return true end
    ```
- 单行执行语句的块可以写在一行，若写成多行，注意缩进
    ```lua
    --good
    if not a then return b end

    --good
    if not a then
        return b
    end
    ```
- 单路`if`判断，可转为 bool 判断，效率可读性均有提高
    ```lua
    -- bad
    if aaa then
        if bbb then
            if ccc then
                if ddd then
                    xxx
                end
            end
        end
    end

    -- good
    if aaa and bbb and ccc and ddd then
        xxx
    end
    ```
- 无限循环使用 `while true do ... end`，不使用 `repeat ... until false`
- 除非必要，一般不用在条件表达式中与 `true`、`false`、`nil` 进行直接判断
    ```lua
    if exp then

    end

    if not exp then

    end

    if type(exp) == "boolean" and exp == true then   -- 明确与true进行判断

    end

    if exp == nil then   -- 明确与nil值进行判断

    end
    ```
- `and` 和 `or` 是 **短路求值**，意味着前面的判断通过，后面的判断表达式不会进行求值，所以不要在多层条件表达式里写逻辑
    ```lua
    if 2 < 1 and 2 < 3 then
        -- 2<1判断为false，后面的 2<3 不会运行
    end

    if 2 > 1 or 2 > 3 then
        -- 2>1判断为true，后面的2>3不会运行
    end
    ```
- 对于多级和超长逻辑判断，用括号与缩进标明层级，逻辑运算符放 **行首**。（当然，能简化逻辑判断最好）
    ```lua
    -- good
    if ((xxx and yyy) or (xxx and zzz)) and 
        xxx or 
        (yyy and zzz) then
        ...
    end

    -- bad
    if ((xxx and yyy) or (xxx and zzz) and xxx) or (yyy and zzz) then
        ...
    end
    ```
- 变量局部化
    - 包含已定义的全局变量比如常用 math, table 等等（大于 2 次以上的使用就认为有必要）
    ```lua
    -- good
    local sin = math.sin
    function localized()
        local x = 0
        for i = 1, 100 do
            x = x + sin(i)
        end
        return x
    end

    -- bad
    function nonlocal()
        local x = 0
        for i = 1, 100 do
            x = x + math.sin(i)
        end
        return x
    end
    ```
    
- 长字符串的拼接建议采用 table 才处理
    ```lua
    -- good
    local s = ''
    local t = {}
    for i = 1,300000 do
        t[#t + 1] = 'a'
    end
    s = table.concat( t, '')

    -- bad
    local s = ''
    local t = {}
    for i = 1,300000 do
        s = s .. 'a'
    end
    ```

- Lua 行尾不建议加上分号，保持 Lua 本身的规范

## 项目规范

- UI编辑器设置生成 lua 文件路径为 `Resource/scripts/app/layout`
- 项目内方法 **分类命名**
    ```lua
    点击事件：onXXXClick();
    网络请求：requestXXX();
    网络响应：onXXXresponse();
    原生回调：onNativeXXX();
    消息回调：onEventXXX();
    ```
- 文件 require 其他模块顺序是 **engine->framework->app**，最后引用同目录模块
    ```lua
    require("engine/xxx/xxx")
    require("framework/xxx/xxx/xxx")
    require("app/base/xxx")
    require("app/xxx")
    ```
- 消息定义统一到 `eventCommand.lua` 文件中，不在直接定义于模块内
- 针对需要立即使用的模块在 **文件头** require 进来，其他的可放在 `runInNextFrame` 中延迟加载
- 对引擎模块的扩展不能直接在引擎层（ engine 目录下）修改，需要写到 **framework** 目录下
- 外部访问某模块内的成员变量需使用 **getXXX，setXXX**，不能直接访问。可使用全局 `addProperty` 函数简化代码实现
- 编辑器以及其他工具生成的配置文件（布局文件，拼图文件）需返回由 **local** 修饰的表
- 针对网络回调中过多的数据传递，可使用 **table** 包裹参数，避免方法定义过长，控制参数的数量
- 函数内使用的数据尽可能通过参数传递进来，而不是直接访问成员变量/全局变量；提高函数本身的模块性
- 项目内存在多个数据类用于模块间共享数据，新增数据需要分清属性放于正确文件中
    ```lua
    globalInfo.lua     -- 整个游戏共享
    accountInfo.lua    -- 账号相关数据，切换账号，数据失效
    roomInfo.lua       -- 房间相关数据， 切换房间，数据失效
    
    clientConfig.lua     -- 客户端相关配置常量
    serverConfig.lua     -- 服务器相关配置常量
    ```
- 不要随意使用 **magic number**，有意义的数字需要用变量/常量定义，至少也要在旁边加上注释
- 布局文件名，应与场景文件名一致最后加 UI 进行区分，如某弹窗名称为：`FileNamePop`，对应布局文件名称应为：`FileNamePopUI`
- 翻译需要注意一下几点：
    ```lua
    1)翻译中出现多个%s加上序号。(原因：多语言翻译有时顺序可能会有改变)
        举例：
        -- bad
        "%s對%s優雅的拋了個真心玫瑰";
        
        -- good
        "#s1對#s2優雅的拋了個真心玫瑰";
        
    2)涉及到日期的文字，%s 也需要加上序号，并于翻译文字后面加上实例注释。(原因，多语言日期顺序与中文不一样)
        -- bad
        "%s月%s日%s:%s"
        
        -- good
        "#s1月#s2日#s3:#s4";  -- 12月1日 12:00
        
    3)避免出现这样一句话拆分的翻译，这样翻译完全无法翻译了，改为一整句.
        --bad
        str_report_msg1                      = "現在比賽"
        str_report_msg2                      = "分鐘，還剩"
        str_report_msg3                      = "名選手，您現在是第"
        str_report_msg4                      = "名。"
        
        --good
        現在比賽#s1分鐘，還剩#s2名選手，您現在是第#s3名。
        
    4)涉及到比赛，排行榜等含有名次的翻译，需要考虑到多语言中名次非纯数字，如：1st，2nd，3rd，4th
        --bad
        "恭喜您获得本场比赛%d名"   -- 不适配字符串
        
        --good
        "恭喜您获得本场比赛%s名"   -- 适配字符串
    
    ```
- XxxManager 作为成员变量时，命名统一为 m_xxxManager 的形式( m_manager 全名，首字母小写)，方便追踪代码。
- Git 提交信息注明类型：
    - feat：新功能（feature）
    - fix：修补 bug
    - docs：文档（ documentation ）
    - style： 格式（不影响代码运行的变动）
    - refactor：重构（即不是新增功能，也不是修改 bug 的代码变动）
    - test：增加测试
    - chore：构建过程或辅助工具的变动
    - cr：code review 修改

