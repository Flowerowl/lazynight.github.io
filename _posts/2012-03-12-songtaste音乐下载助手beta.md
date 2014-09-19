---
title: SongTaste音乐下载助手(Beta)
author: Flowerowl
layout: post
permalink: /1586.html
duoshuo_thread_id:
  - 1266981
views:
  - 1389
bot_views:
  - 2
categories:
  - 作品
tags:
  - 作品
---
  
大家肯定都用过<span style="color: #339966;"><a href="http://www.duanmu.org/" target="_blank"><span style="color: #339966;">木头</span></a></span>写的Songtaste音乐下载助手吧，反正我很喜欢，高中在st听得大部分歌曲就靠它了。

高考毕业之后找到木头QQ，加上请教了一番，那时还不懂什么是vb，什么是winsock，什么是xmlhttp。

木头也不吝惜的给我讲了很多，当时我是受宠若惊。

（其实我建网站其中原因之一也是因为他，那时候我问他皮肤是谁写的，他说自己写的，3天时间，我当时是把他当神看的&#8230;）

时隔两年，自己也写了一个仿品，功能有限，并且不稳定。

规则：只能下载歌单，例如：<span style="color: #339966;"><a href="http://www.songtaste.com/playmusic.php?song_id=95034,559852,190360,817134,1018586,2986867,305934,783740,2879058,2960337"><span style="color: #339966;">http://www.songtaste.com/playmusic.php?song_id=95034,559852,190360,817134,1018586,2986867,305934,783740,2879058,2960337</span></a></span>

单曲不支持下载。

[<img class="aligncenter size-full wp-image-1588" title="songtaste" src="http://lazynight.me/wp-content/uploads/2012/03/songtaste1.gif" alt="" width="372" height="483" />][1]

原理：

分析html文件，并对字符串进行处理，抓取歌曲ID,TITILE, SINGER,URL.

利用webhttprequest以及webhttpresponse进行请求，此处没有用downloadfile，直接利用stream。

目前有几个bug，请求速度慢，不稳定的时候获取不了下载列表，下载请求被服务器拒绝，模拟user-agent也无济于事，偶尔会down掉。

此程序昨天下午四点开始萌发念头，晚6点开写，今天改了一天的bug，未遂，暂停折腾了。

<span style="color: #339966;"><a href="http://dl.dbank.com/c0qace48qn" target="_blank"><span style="color: #339966;">下载程序</span></a></span>

转载请注明：[于哲的博客][2] &raquo; [SongTaste音乐下载助手(Beta)][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/03/songtaste1.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/1586.html