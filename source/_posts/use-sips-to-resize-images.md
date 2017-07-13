---
title: Mac 使用 sips 操作你的图片
date: 2017-07-13 16:12:10
categories: Tips
description: 在 Mac OSX 下，你是否知道图片操作利器 sips，今天我们就来掀开她的面纱？
tags: [Mac, sips, skill]
---

## 前言

使用 Mac OSX 的过程中，发现需要调整图片的大小，以前在 windows 都是采用格式工厂或者 PhotoShop 等工具进行修改，当然我也用 Python 写过一个脚本，[看这里](http://amnon.win/2017/07/13/use-python-to-resize-images/)。但后面发现在 Mac 下有更方便处理的方式，喜出望外。

## Sips

没错，这款利器就是 [Spis](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/sips.1.html)， 拥有了它，妈妈再也不用担心我对图片进行操作了。

查看一下 sips 的 API：

```bash
~ sips --help
sips 10.4.4 - scriptable image processing system.
This tool is used to query or modify raster image files and ColorSync ICC profiles.
Its functionality can also be used through the "Image Events" AppleScript suite.

  Usages:
    sips [-h, --help]
    sips [-H, --helpProperties]

    sips [image-query-functions] imagefile ...

    sips [profile-query-functions] profile ...

    sips [image modification functions] imagefile ...
         [--out result-file-or-dir]

    sips [profile modification functions] profile ...
         [--out result-file-or-dir]


  Profile query functions:
    -g, --getProperty key
    -X, --extractTag tag tagFile
    -v, --verify

  Image query functions:
    -g, --getProperty key
    -x, --extractProfile profile

  Profile modification functions:
    -s, --setProperty key value
    -d, --deleteProperty key
        --deleteTag tag
        --copyTag srcTag dstTag
        --loadTag tag tagFile
        --repair

  Image modification functions:
    -s, --setProperty key value
    -d, --deleteProperty key
    -e, --embedProfile profile
    -E, --embedProfileIfNone profile
    -m, --matchTo profile
    -M, --matchToWithIntent profile intent
        --deleteColorManagementProperties
    -r, --rotate degreesCW
    -f, --flip horizontal|vertical
    -c, --cropToHeightWidth pixelsH pixelsW
    -p, --padToHeightWidth pixelsH pixelsW
        --padColor hexcolor
    -z, --resampleHeightWidth pixelsH pixelsW
        --resampleWidth pixelsW
        --resampleHeight pixelsH
    -Z, --resampleHeightWidthMax pixelsWH
    -i, --addIcon
    -o, --optimizeColorForSharing
```

## 常用的操作

```bash
# 指定宽高输出图片
sips --out output.png -z ${height} ${width} input.png

# 制定宽度、保持比例输出图片
sips --out output.png --resampleWidth ${width} input.png

# 指定宽度、保持比例(会覆盖源图片)
sips --resampleWidth ${width} *.jpg

# 指定宽高(会覆盖源图片)
sips -z ${height} ${width} *.jpg

# 顺时针旋转 90 度
sips -r 90 image_file_name

# 水平翻转图片 
# horizontal/vertical 水平／垂直
sips -f horizontal image_file_name

# 修改图片格式
# 使用 -s 参数可以修改图片格式为指定值，sips 支持 jpeg | tiff | png | gif | jp2 | pict | bmp | qtif | psd | sgi | tga 共 11 种格式。
sips -s format jpeg input.png -o output.jpg

# 获取图片 meta 信息
sips -g pixelWidth -g pixelHeight image_file_name

# 可使用查看更多
man sips

```

## 总结

有了 `sips` 祝你在图片操作上越走越远。

