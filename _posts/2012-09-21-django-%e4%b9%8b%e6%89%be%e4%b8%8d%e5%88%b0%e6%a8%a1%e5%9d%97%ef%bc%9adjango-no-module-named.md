---
title: 'Django 之找不到模块：django no module named ***'
author: Flowerowl
layout: post
permalink: /2498.html
duoshuo_thread_id:
  - 8798086
views:
  - 1299
bot_views:
  - 2
categories:
  - Django
  - 技术杂谈
---
先约定好，项目为mysite，应用为blog

在mysite文件夹下，有一个settings.py

在INSTALLED_APP下不要写mysite.blog,直接添加blog即可，这是python 与 Django 版本不兼容导致。

同理，在所有导入blog模块的文件里，不要写  import mysite.blog，直接写import blog 即可。

折腾半天，终于折腾出来了。

[<img class="alignnone size-full wp-image-2499" title="django" src="http://lazynight.me/wp-content/uploads/2012/09/django.png" alt="" width="338" height="242" />][1]

转载请注明：[于哲的博客][2] &raquo; [Django 之找不到模块：django no module named \***][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/09/django.png
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2498.html