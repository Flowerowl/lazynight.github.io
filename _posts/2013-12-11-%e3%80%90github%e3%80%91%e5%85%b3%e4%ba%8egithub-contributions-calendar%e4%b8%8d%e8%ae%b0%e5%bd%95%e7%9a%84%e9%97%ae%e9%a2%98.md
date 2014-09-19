---
title: 【Github】关于github Contributions Calendar不记录的问题
author: Flowerowl
layout: post
permalink: /3304.html
views:
  - 926
duoshuo_thread_id:
  - 1220743779864322547
categories:
  - Git
  - 技术杂谈
tags:
  - Calendar
  - Contributions
  - Github
  - 动态
  - 日志
  - 记录
---
最近一直在github更新资源，但是发现calendar一直不给我更新提交的动态，容易么我&#8230;

<img title="NewImage.png" src="http://lazynight.me/wp-content/uploads/2013/12/NewImage2.png" alt="NewImage" width="163" height="169" border="0" />

看着白花花的小空格，伤心

坚决要处理一下这个关乎我职业生涯的问题！！

搜了一下，问题来源就是：github的邮箱和你本地的配置邮箱不一样···

查看本地git邮箱：

<pre class="command-line" style="margin: 10px 0px; padding: 10px; border: 2px solid #dddddd; font-family: Monaco, 'DejaVu Sans Mono', 'Courier New', monospace; font-size: 13px; line-height: inherit; vertical-align: baseline; border-top-left-radius: 3px; border-top-right-radius: 3px; border-bottom-right-radius: 3px; border-bottom-left-radius: 3px; -webkit-background-clip: padding-box; background-clip: padding-box; color: #ffffff; -webkit-font-smoothing: auto; overflow: auto; background-color: #333333;"><span class="command" style="margin: 0px; padding: 0px; border: 0px; font-family: inherit; font-size: inherit; font-style: inherit; font-variant: inherit; line-height: inherit; vertical-align: baseline; white-space: pre-wrap;">git config user.email</span></pre>

然后在github里的accout settings》email里看看你的primary github email 是不是你本地那个邮箱。

改一下就好了

<pre class="command-line" style="margin: 10px 0px; padding: 10px; border: 2px solid #dddddd; font-family: Monaco, 'DejaVu Sans Mono', 'Courier New', monospace; font-size: 13px; line-height: inherit; vertical-align: baseline; border-top-left-radius: 3px; border-top-right-radius: 3px; border-bottom-right-radius: 3px; border-bottom-left-radius: 3px; -webkit-background-clip: padding-box; background-clip: padding-box; color: #ffffff; -webkit-font-smoothing: auto; overflow: auto; background-color: #333333;"><span class="command" style="margin: 0px; padding: 0px; border: 0px; font-family: inherit; font-size: inherit; font-style: inherit; font-variant: inherit; line-height: inherit; vertical-align: baseline; white-space: pre-wrap;">git config --global user.email "<em style="margin: 0px; padding: 0px; border: 0px; font-family: inherit; font-size: inherit; font-variant: inherit; line-height: inherit; vertical-align: baseline; color: #f9fe64;">me@here.com</em>"</span></pre>

参考文章：<https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile>

<https://help.github.com/articles/why-are-my-commits-linked-to-the-wrong-user>

转载请注明：[于哲的博客][1] &raquo; [【Github】关于github Contributions Calendar不记录的问题][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3304.html