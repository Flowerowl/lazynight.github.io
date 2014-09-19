---
title: 修改Mac系统截图文件类型，保存路径
author: Flowerowl
layout: post
permalink: /2720.html
views:
  - 778
duoshuo_thread_id:
  - 1220743779864322422
bot_views:
  - 2
categories:
  - 实用
tags:
  - mac
  - QQ
  - 截图
---
最近看了一下QQ截图，默认为png格式，比较大，和系统截图对比了一下，都是png格式，系统截图比QQ截图还大。

百度之余发现可以更改系统截图默认文件类型，还有默认保存路径。

命令如下：

更改文件类型

> defaults write com.apple.screencapture type jpg

以后的截图就都是jpg格式的了。

更改保存路径

> defaults write com.apple.screencapture location <path>

以后的截图都会放到你指定的目录，path太长不好输入？命令defaults write com.apple.screencapture location 打到这里以后，直接从finder把目录拖进terminal窗口即可。

另外推荐一款高质量图像压缩的小工具：ImageOptim。

截图命令：command + shift + 4是指定区域 （最常用），command + shift + 3是全屏。

转载请注明：[于哲的博客][1] &raquo; [修改Mac系统截图文件类型，保存路径][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2720.html