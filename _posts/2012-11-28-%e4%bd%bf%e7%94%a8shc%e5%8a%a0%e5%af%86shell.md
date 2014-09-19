---
title: 使用shc加密shell
author: Flowerowl
layout: post
permalink: /2726.html
views:
  - 1116
duoshuo_thread_id:
  - 1220743779864322425
bot_views:
  - 1
categories:
  - Linux
  - 安全
  - 工具
tags:
  - Linux
  - shc
  - shell
  - 加密
---
刚考完试用户界面设计，放松一下，看个小视频~使用shc加密shell~ok，折腾试试。

> shc是一个脚本编译工具, 使用RC4加密算法, 它能够把shell程序转换成二进制可执行文件(支持静态链接和动态链接)。  
> shc官网:http://www.datsi.fi.upm.es/~frosal/

#### 用途：

当我们写的shell脚本，存在有敏感信息如账号密码，于是想加强脚本的安全性；还有不想让别人查看/修改您的shell核心代码等等情况。都可使用以下工具进行加密。

#### 使用方法：

tar vxf shc-3.8.9.tgz

cd shc-3.8.9

修改shc-3.8.9.c  为 shc.c

make

成功后会显示：

sh-3.2# make  
cc -Wall shc.c -o shc  
\*** ?Do you want to probe shc with a test script?  
\*** Please try&#8230; make test

然后我们编写一个简单的脚本

vim 1.sh

内容为：

#!/bin/sh  
echo &#8220;Welcome to Lazynight.&#8221;  
echo &#8220;By 夜阑&#8221;

保存退出

常用参数：  
-e date （指定过期日期）  
-m message （指定过期提示的信息）  
-f script_name（指定要编译的shell的路径及文件名）  
-r   Relax security. （可以相同操作系统的不同系统中执行）  
-v   Verbose compilation（编译的详细情况）

使用方法：  
shc -v -f abc.sh  
-v 是现实加密过程  
-f 后面跟需要加密的文件   
运行后会生成两个文件:  
abc.sh.x 和 abc.sh.x.c

运行如下：

<img title="shc.gif" src="http://lazynight.me/wp-content/uploads/2012/11/shc.gif" alt="Shc" width="600" height="379" border="0" />

转载请注明：[于哲的博客][1] &raquo; [使用shc加密shell][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2726.html