---
title: ipad玩渗透之metasploit
author: Flowerowl
layout: post
permalink: /2461.html
duoshuo_thread_id:
  - 1220743779862923201
views:
  - 1482
bot_views:
  - 2
categories:
  - 安全
  - 工具
  - 渗透
tags:
  - metasploit
  - 安全
  - 框架
  - 渗透
---
多半个月没冒泡了，哈哈，大多时间是在看书，其实看书也挺爽的。

其中呢，就玩了一下metasploit，推荐《Metasploit渗透测试指南》，貌似网上只有试读（中文版），英文版倒是全书就有，不过着实难啃，还是花点钱买本吧。

前几天就想在ipad上部署一些crack工具，无奈数量甚少，不过没关系，有了metasploit，我觉得差不多够用了。

跟我来~进行安装吧~

<span style="color: #ff0000;"><a href="http://dl.vmall.com/c0uqk055kg" target="_blank"><span style="color: #ff0000;">打包下载</span></a></span>

1.安装ruby\_1.8.6-p111-5\_iphoneos-arm.deb

rubygems\_1.2.0-3\_iphoneos-arm.deb

注意：不要在cydia中安装最新版本的ruby，metasploit会不兼容。

（小计：找这个rubygems找了半天，apt.saurik.com被墙了，真搞不懂连这个也被墙，后来翻墙下载的，各位就不用麻烦了）

2.安装framework-4.0.0.tar.bz2

cd /private/var/

tar jxpf framework-4.0.0.tar.bz2

会把框架安装在msf3下。

3.如果你还想安装点别的，比如nmap，whois，就自行下载吧。

进入msf3下，

./msfconsole

运行如下：

[<img class="alignnone size-full wp-image-2462" title="metasploit" src="http://lazynight.me/wp-content/uploads/2012/09/metasploit.png" alt="" width="1023" height="766" />][1]

转载请注明：[于哲的博客][2] &raquo; [ipad玩渗透之metasploit][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/09/metasploit.png
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2461.html