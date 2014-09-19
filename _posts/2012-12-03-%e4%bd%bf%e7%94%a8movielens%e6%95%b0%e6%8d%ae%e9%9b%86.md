---
title: 使用MovieLens数据集
author: Flowerowl
layout: post
permalink: /2744.html
views:
  - 2132
duoshuo_thread_id:
  - 1220743779864322432
bot_views:
  - 1
categories:
  - 数据挖掘
tags:
  - MovieLens
  - 人工智能
  - 数据挖掘
  - 数据集
  - 机器学习
---
这节我们使用MovieLens的大数据集，一个涉及电影评价的真实数据集。

下载地址：http://www.grouplens.org/node/12

里边有很多文件，不过我们只需要u.item还有u.data。

<img title="udata.png" src="http://lazynight.me/wp-content/uploads/2012/12/udata.png" alt="Udata" width="345" height="188" border="0" />

格式为：用户ID，电影ID，评分，评价时间

我们便携一个函数来获取格式化的列表：

<pre>def loadMovieLens(path='/Users/Flowerowl/intelligence/lib/ml-100k'):
	movies = {}
	for line in open(path+'/u.item'):
		(id, title) = line.split('|')[0:2]
		movies[id] = title

	prefs = {}
	for line in open(path + '/u.data'):
		(user, movieid, rating, ts) = line.split('\t')
		prefs.setdefault(user,{})
		prefs[user][movies[movieid]] = float(rating)
	return prefs
	
</pre>

转载请注明：[于哲的博客][1] &raquo; [使用MovieLens数据集][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2744.html