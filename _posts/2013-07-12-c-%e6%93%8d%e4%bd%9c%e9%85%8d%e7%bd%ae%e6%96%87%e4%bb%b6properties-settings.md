---
title: 'C# 操作配置文件Properties.Settings'
author: Flowerowl
layout: post
permalink: /2934.html
views:
  - 1231
duoshuo_thread_id:
  - 1220743779864322480
bot_views:
  - 3
categories:
  - 'C#'
  - 技术杂谈
tags:
  - C
  - Settings
---
最近用C#搭个管理系统的大致框架，好久不用.NET，手都生了。

=====================================

1、定义

在Settings.settings文件中定义配置字段。把作用范围定义为：User则运行时可更改，Applicatiion则运行时不可更改。可以使用数据网格视图，很方便；

2、读取配置值

text1.text = Properties.Settings.Default.FieldName;  
//FieldName是你定义的字段

3、修改和保存配置

Properties.Settings.Default.FieldName = &#8220;server&#8221;;  
Properties.Settings.Default.Save();//使用Save方法保存更改

<wbr />

注意：当设置scope为User时他的配置放在 C:\Documents and Settings\LocalService\Local Settings\Application Data\在这个目录下或子目录user.config 配置文件中。

转载请注明：[于哲的博客][1] &raquo; [C# 操作配置文件Properties.Settings][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2934.html