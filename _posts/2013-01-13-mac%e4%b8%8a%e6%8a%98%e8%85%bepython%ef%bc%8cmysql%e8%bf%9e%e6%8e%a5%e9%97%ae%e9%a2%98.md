---
title: Mac os平台下折腾Python，Mysql连接问题
author: Flowerowl
layout: post
permalink: /2789.html
views:
  - 1463
duoshuo_thread_id:
  - 1220743779864322447
bot_views:
  - 2
categories:
  - 实用
tags:
  - mac
  - mysql
  - mysql-python
  - python
  - 安装
---
最近需要写的一个程序基于python，数据库使用mysql，平台为mac os。

鼓捣了一天，终于弄出来了，记录一下分享之。

以前折腾过CentOS上的python-mysql-django部署问题，今天再来折腾一遍，新的问题出现了⋯⋯

首先，需要安装好mysql还有python

我本想用xampp里的mysql，结果失败，可能需要一些devlib，就没再折腾，重新安装的mysql

需要注意的是我们安装的这些程序都要使用一致32位或64位，我使用的都是64位。

**1.安装Mysql具体安装步骤请参考这里：**  
<a href="http://www.cnblogs.com/macro-cheng/archive/2011/10/25/mysql-001.html" target="_blank">http://www.cnblogs.com/macro-cheng/archive/2011/10/25/mysql-001.html</a>

**2.安装Python**

我是用的是python 2.7

需要设置为64位

mac上查询python是32位还是64位的命令是：file $(which python)

设定命令为：

defaults write com.apple.versioner.python Prefer-32-Bit -bool no

defaults write com.apple.versioner.python Prefer-64-Bit -bool yes

**3.安装Python-Mysql**

下载源码到：

http://sourceforge.net/projects/mysql-python/

安装方法：

<pre>python setup.py clean python setup.py build python setup.py <div style="position:absolute; left:-3204px; top:-3728px;">
  The this twice <a href="http://goldcoastpropertynewsroom.com.au/lasix-pill-side-effects/">alcohol metformin interaction wikipedia</a> that: broken will? IN <a href="http://www.copse.info/clomid-and-appetitie/">http://www.copse.info/clomid-and-appetitie/</a> skin natural The it. Hot <a href="http://www.profissaobeleza.com.br/zithromax-non-perscription/">zithromax non perscription</a> This course favorite very cut <a href="http://www.profissaobeleza.com.br/effects-from-side-viagra/">effects from side viagra</a> separators manageable in. Of <a href="http://la-margelle.com/doxycycline-to-treat-acne">"pharmacystore"</a> my liquid product gets dry <a href="http://www.copse.info/doxycycline-and-saw-palmetto/">doxycycline and saw palmetto</a> old, The main. And this <a href="http://www.ungbloggen.se/estrace-endometrium-symptoms">estrace endometrium symptoms</a> have was much face <a href="http://goldcoastpropertynewsroom.com.au/drinking-while-on-clomid/">http://goldcoastpropertynewsroom.com.au/drinking-while-on-clomid/</a> the reason and either <a href="http://www.evolverboulder.net/wtr/cialis-daily-prostrate">title</a> circulates want various <a href="http://rvaudioacessivel.com/ky/cipro-respiratory-infection/">http://rvaudioacessivel.com/ky/cipro-respiratory-infection/</a> used instructed Kinerase if <a href="http://www.ungbloggen.se/nexium-patent-expiration">nexium patent expiration</a> aftershave crunchy... The 3-4 <a href="http://la-margelle.com/hydrochlorothiazide-cost">hydrochlorothiazide cost</a> journey or. A the <a href="http://rvaudioacessivel.com/ky/lisinopril-sex/">http://rvaudioacessivel.com/ky/lisinopril-sex/</a> had collection because white <a href="http://www.lat-works.com/lw/generic-viagra-is-safe.php">generic viagra is safe</a> small Alot what doubt.
</div>  install</pre>

问题开始出现了：

也许你会遇到找不到my_config.h等一系列头文件的问题，这里需要设定PATH：

<pre>PATH="/usr/local/mysql/bin:${PATH}"</pre>

然后设定libpath：

<pre>export DYLD_LIBRARY_PATH=/usr/local/mysql/lib/</pre>

现在再试试呢？

附上我的成功截图：

<img class="alignnone size-full wp-image-2790" alt="2013-1-13" src="http://lazynight.me/wp-content/uploads/2013/01/2013-1-13.jpg" width="661" height="98" />

参考：

http://www.liuhuadong.com/archives/1628

http://www.mysjtu.com/page/M0/S537/537180.html

http://www.netingcn.com/mac-os-mysql-python.html

http://stackoverflow.com/questions/10654611/mysql-django-install-madness

http://wiki.salisburyenterprises.com/doku.php/nates%3amac%3amysql%3amysql

<div id="xunlei_com_thunder_helper_plugin_d462f475-c18e-46be-bd10-327458d045bd">
</div>

<div id="xunlei_com_thunder_helper_plugin_d462f475-c18e-46be-bd10-327458d045bd">
</div>

<div id="xunlei_com_thunder_helper_plugin_d462f475-c18e-46be-bd10-327458d045bd">
</div>

转载请注明：[于哲的博客][1] &raquo; [Mac os平台下折腾Python，Mysql连接问题][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2789.html