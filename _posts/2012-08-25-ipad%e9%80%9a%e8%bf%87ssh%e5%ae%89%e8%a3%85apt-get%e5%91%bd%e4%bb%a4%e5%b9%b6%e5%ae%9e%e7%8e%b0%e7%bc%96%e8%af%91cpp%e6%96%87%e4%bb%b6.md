---
title: ipad 进行ssh，安装apt-get命令并实现编译cpp文件
author: Flowerowl
layout: post
permalink: /2442.html
duoshuo_thread_id:
  - 6958858
views:
  - 1347
bot_views:
  - 3
categories:
  - 杂记
tags:
  - apt
  - ipad
---
之前我用的是离线的方法安装的各种程序，其实，只要终端有了apt命令，完全可以像linux一样实现deb安装。

本文基于之前一片文章：http://lazynight.me/2429.html

首先在cydia中安装openssh，或者自己下载openssh的deb包进行安装。

我在macbook上用的是终端来连接ipad，方法：

打开终端，输入 ssh -p 端口 用户名@IP，回车

端口默认22，ip即为ipad  网络ip。

[<img class="alignnone size-full wp-image-2443" title="ssh" src="http://lazynight.me/wp-content/uploads/2012/08/ssh.jpg" alt="" width="567" height="114" />][1]

注意，直接用root用户可能等不进去，可以试试mobile。

deb准备：apt7-lib\_0.7.25.3-9\_iphoneos-arm.deb

apt7\_0.7.25.3-6\_iphoneos-arm.deb

apt7-ssl\_0.7.25.3-3\_iphoneos-arm.deb

apt7-key\_0.7.25.3-3\_iphoneos-arm.deb

apt0.6\_Transitional\_1\_0-23\_iphoneos-arm.deb

<span style="color: #ff0000;"><a href="http://dl.vmall.com/c0kf9nhvba" target="_blank"><span style="color: #ff0000;">打包下载</span></a></span>

安装方法参考上面那片文章。

安装完毕可以去终端试一下apt-get ,如果apt 0.7.25.3 for iphoneos-arm compiled on Feb 24 2010 09:59:20 类似的文字说明安装成功。

现在，可以来装一下g++，用来编译cpp文件。

命令：apt-get install g++

ok，看看g++安装完成没有呢，输入 g++ -v，可以查看相应版本。

编译步骤和gcc一样: g++ hello.cpp -o hello

然后签名，然后运行。

ldid -S hello

./hello

转载请注明：[于哲的博客][2] &raquo; [ipad 进行ssh，安装apt-get命令并实现编译cpp文件][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/ssh.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2442.html