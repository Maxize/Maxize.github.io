---
title: 使用 Python 批量调整图片大小
date: 2017-07-13 16:36:08
categories: Python
description: 解放生产力之 Python 调整图片大小
tags: [Python, Pilliow, Image]
---

## 前言

之前拿到表情图片的时候发现给的尺寸都偏大，而实际使用又不需要那么大，如果人工一个个去改，感觉要累死的节奏，正好用 Python 玩的有点上火，于是就去找了一下，发现 Python 下有一个 Pillow 图片库可以很方便的处理，于是就有了下面的一个脚本。

_Python 有毒，请慎用！_

## 使用

注意： 需要手动安装 Pillow 库，具体可以 [Google 之](https://www.google.com.sg/search?q=%E5%AE%89%E8%A3%85+python+image&oq=%E5%AE%89%E8%A3%85+python+image&gs_l=serp.3..0i8i30k1l2.9538.9538.0.9793.1.1.0.0.0.0.145.145.0j1.1.0....0...1.1.64.serp..0.1.145.sMXstBq2wgM) 。

Mac 下可以直接使用：
```bash
# Pillow 依赖于 multiprocessing
sudo pip install multiprocessing
sudo pip install Pillow
```

下面是具体的代码：

```python
#!/usr/bin/python
# coding:utf8
# 使用 Image 修改宽高
# usage: python resizePic.py
# the file or filePath: /Users/amnon/Documents/test.png
# the width you want: 120
# the height you want: 120

import os
from PIL import Image

# 返回指定目录的所有文件（包含子目录的文件）
def get_all_pic(floder_path):
    file_list = []
    if floder_path is None:
        raise Exception("floder_path is None")
    for dirpath, dirnames, filenames in os.walk(floder_path):
        for name in filenames:
            file_list.append(dirpath + os.sep() + name)
    return file_list

def resize_all_pic(file_list, width, height):
    size = 0
    for pic in file_list:
        img = Image.open(pic)
        w,h = img.size
        new_img = img.resize((int(width),int(height)),Image.ANTIALIAS)
        new_img.save(pic,quality=100)

def main():
    filePath = raw_input("the file or filePath: ")
    width    = raw_input("the width you want: ")
    height   = raw_input("the height you want: ")
    file_list = []
    if os.path.isfile(filePath):
        file_list = [filePath]
    else:
        file_list = get_all_pic(filePath)
    resize_all_pic(file_list,width, height)

if __name__ == '__main__':
    main()
```

## 总结

使用 Python 是解放生产力的第一步，祝你在这条路上越走越远。
