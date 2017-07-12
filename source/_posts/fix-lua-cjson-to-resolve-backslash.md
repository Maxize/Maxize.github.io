---
title: 解决 Lua CJSON 无法解析单反斜杠 \ 和反斜杠加单引号 \'
date: 2017-07-10 11:06:59
categories: Bugs
description: 项目在使用 Lua CJSON 是遇到这样一个 string `“\\\' source”`而无法正常解析，为了解决这个问题，有了下面这篇文章。
tags: Lua CJSON
---

## 背景

项目组一直用 [Lua CJSON](https://github.com/mpx/lua-cjson) 解析 Json，效率上也比 Lua 本身的 Json 库效率高不少，解析的东西也很多，但最近遇到 `"\\"` 或者 `"\\\'"` 这样符合转义规则的字符串却解析不出来，于是硬着头皮去看了 Lua CJSON 的源码，并尝试修复。

## 修改

通过阅读源代码，我发现 Lua CJSON 解析字符串的逻辑集中在这里。

```c++
static void json_next_string_token(json_parse_t *json, json_token_t *token)
{
    char *escape2char = json->cfg->escape2char;
    char ch;

    /* Caller must ensure a string is next */
    assert(*json->ptr == '"');

    /* Skip " */
    json->ptr++;

    /* json->tmp is the temporary strbuf used to accumulate the
     * decoded string value.
     * json->tmp is sized to handle JSON containing only a string value.
     */
    strbuf_reset(json->tmp);

    while ((ch = *json->ptr) != '"') {
        if (!ch) {
            /* Premature end of the string */
            json_set_token_error(token, json, "unexpected end of string");
            return;
        }

        /* Handle escapes */
        if (ch == '\\') {
            /* Fetch escape character */
            ch = *(json->ptr + 1);

            /* Translate escape code and append to tmp string */
            ch = escape2char[(unsigned char)ch];
            if (ch == 'u') {
                if (json_append_unicode_escape(json) == 0)
                    continue;

                json_set_token_error(token, json,
                                     "invalid unicode escape code");
                return;
            }
            if (!ch) {
                json_set_token_error(token, json, "invalid escape code");
                return;
            }

            /* Skip '\' */
            json->ptr++;
        }
        /* Append normal character or translated single character
         * Unicode escapes are handled above */
        strbuf_append_char_unsafe(json->tmp, ch);
        json->ptr++;
    }
    json->ptr++;    /* Eat final quote (") */

    strbuf_ensure_null(json->tmp);

    token->type = T_STRING;
    token->value.string = strbuf_string(json->tmp, &token->string_len);
}
```

解析字符串是通过 `"` 来标示的，然后反斜杠开头进入逻辑判断，目前 Lua CJSON 支持的几个关键转义符有：在 `function json_create_config` 中

```c++
    /* Lookup table for parsing escape characters */
    for (i = 0; i < 256; i++)
        cfg->escape2char[i] = 0;          /* String error */
    cfg->escape2char['"'] = '"';
    cfg->escape2char['\\'] = '\\';
    cfg->escape2char['/'] = '/';
    cfg->escape2char['b'] = '\b';
    cfg->escape2char['t'] = '\t';
    cfg->escape2char['n'] = '\n';
    cfg->escape2char['f'] = '\f';
    cfg->escape2char['r'] = '\r';
    cfg->escape2char['u'] = 'u';          /* Unicode parsing required */
```

而在处理上面提到的情况的时候，因为转义符是一个特殊的字符，检测到转移符就会往下去检索下一个字符，只有符合的才会继续处理，不符合的就不继续往下处理，所以拿到的是空的。为了不对原来的改动造成大影响，我们就把原来解析出错 `return` 的改成原样返回。

改动如下：

```c++
static void json_next_string_token(json_parse_t *json, json_token_t *token)
{
    char *escape2char = json->cfg->escape2char;
    char ch;

    /* Caller must ensure a string is next */
    assert(*json->ptr == '"');

    /* Skip " */
    json->ptr++;

    /* json->tmp is the temporary strbuf used to accumulate the
     * decoded string value.
     * json->tmp is sized to handle JSON containing only a string value.
     */
    strbuf_reset(json->tmp);

    while ((ch = *json->ptr) != '"') {
        if (!ch) {
            /* Premature end of the string */
            json_set_token_error(token, json, "unexpected end of string");
            return;
        }

        /* Handle escapes */
        if (ch == '\\') {
            /* Fetch escape character */
            // modify 1 begin
            // 使用临时变量 tempChar 存储读取到的字符
            char tempChar = *(json->ptr + 1);

            /* Translate escape code and append to tmp string */
            tempChar = escape2char[(unsigned char)tempChar];
            if (tempChar == 'u') {
                if (json_append_unicode_escape(json) == 0)
                    continue;

                json_set_token_error(token, json,
                                     "invalid unicode escape code");
                return;
            }
            // modify 1 end
            // modify 2 begin 
            // 判断如果不在定义的列表中，指针往前走，不要返回错误
            if (tempChar) {
                ch = tempChar;
                json->ptr++;
            }
            // modify 2 end
        }
        /* Append normal character or translated single character
         * Unicode escapes are handled above */
        strbuf_append_char_unsafe(json->tmp, ch);
        json->ptr++;
    }
    json->ptr++;    /* Eat final quote (") */

    strbuf_ensure_null(json->tmp);

    token->type = T_STRING;
    token->value.string = strbuf_string(json->tmp, &token->string_len);
}
```

这样 Lua CJSON 就能处理我们需要的了。

## 总结

在使用第三方库出现问题的时候要深入内部去寻找问题，而通过修改这个也能更好的了解原理。如果你也出现类似的情况，希望可以帮到你。
