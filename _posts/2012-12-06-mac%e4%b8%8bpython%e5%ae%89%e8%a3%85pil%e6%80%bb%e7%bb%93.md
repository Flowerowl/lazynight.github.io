---
title: Mac下Python安装PIL总结
author: Flowerowl
layout: post
permalink: /2760.html
views:
  - 2120
duoshuo_thread_id:
  - 1220743779864322440
bot_views:
  - 2
categories:
  - 实用
  - 技术杂谈
tags:
  - Macbook
  - PIL
  - python
  - 安装
---
最近需要用python绘制一幅图像，用到了PIL，安装了十几次，最后成功，记一下~

关于PIL：

官方网站：http://www.pythonware.com/products/pil/

> PIL使你可以通过Python解释器进行图像处理. 她支持多种文件格式，提供了强大的图像处理能力。

我遇到的错误是：

IOError: encoder jpeg not available

原因：

错误信息说明这个问题应该是跟jpg图片的处理有关的，说明python的PIL库出问题了。PIL安装n回了，google一番之后，得知这个问题的原因是PIL的jpg图片支持组件没有安装导致的。

需要组件：

jpeglib：http://www.ijg.org/files/

littlecms：http://www.littlecms.com/

FreeType2库：http://www.freetype.org/

解决办法：

1.删除已经安装的PIL：

> $ rm -rf /usr/lib/python2.5/site-packages/PIL
> 
> $ rm /usr/lib/python2.5/site-packages/PIL.pth

2.安装JPEG库和FreeType2库

3.编译PIL:

> $ python setup.py build_ext –i

4.安装PIL

如果出现以下情况，则成功。

&#8212; TKINTER support available  
&#8212; JPEG support available  
&#8212; ZLIB (PNG/ZIP) support available  
&#8212; FREETYPE2 support available  
&#8212; LITTLECMS support available  
&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;-

具体请看：

http://stackoverflow.com/questions/5345348/what-is-the-best-way-to-install-pil-on-mac-snow-leopard-with-xcode-4-installed

将下载的三个压缩包解压，进行安装。

转载请注明：[于哲的博客][1] &raquo; [Mac下Python安装PIL总结][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2760.html