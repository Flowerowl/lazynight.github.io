---
title: 构建一个基于del.icio.us的链接推荐系统
author: Flowerowl
layout: post
permalink: /2740.html
views:
  - 1639
duoshuo_thread_id:
  - 1220743779864322430
bot_views:
  - 3
categories:
  - 数据挖掘
tags:
  - delicious
  - 个性化推荐
  - 数据挖掘
  - 链接
---
这一节介绍从delicious，一个在线书签网站获取数据，利用这些数据查找品味相似的用户，并向他们推荐未曾看过的链接。

关于[delicious][1]：

> 同flickr一样，delicious均为标签类站点，一个帮助用户共享他们喜欢网站链接的流行网站。标签的作用就是可以让人们对某一条目进行标注，如 添加词语以及短语。但就delicious而言，所进行标注的条目换成了书签。Delicious提供了一种简单共享网页的方法，它为无数互联网用户提供共享及分类他们喜欢的网页书签。

<img title="delicious.png" src="http://lazynight.me/wp-content/uploads/2012/12/delicious.png" alt="Delicious" width="600" height="371" border="0" />

使用Delicious的API：

http://code.google.com/p/pydelicious/downloads/list

下载pydelicious.py

运行需要事先安装setuptools还有feedparse，请确保python的这两个工具包安装完毕。

运行如下：

>>> import pydelicious  
>>> pydelicious.get_popular(tag=&#8217;music&#8217;)

得到一组近期张贴的有关music的热门链接

[{'count': '', 'extended': u'', 'hash': '', 'description': u'nakkheeran in', 'tags': u'write', 'href': u'http://nakkheeran.in/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'rionegro com ar', 'tags': u'humor', 'href': u'http://rionegro.com.ar/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'esposasymaridos com', 'tags': u'loanspayday', 'href': u'http://esposasymaridos.com/', 'user': u'muzyleninun', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'bmw ca', 'tags': u'art', 'href': u'http://bmw.ca/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'selfseo info', 'tags': u'blogs', 'href': u'http://selfseo.info/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'casino zia park hobbs nm', 'tags': u'blogs', 'href': u'http://223.easylooklikek.com/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'casino morongo job fair', 'tags': u'Lifestyle', 'href': u'http://107.fastseekingonse.com/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u"Pok\xe9Beach.com, with 'Pokemon Black' and 'Pokemon White', Reshiram, Zekrom, Zoroark, Zorua, & 'HS - Undaunted' set info; your ultimate Pok\xe9mon TCG and Pok\xe9mon news site!", 'tags': u'pokemon', 'href': u'http://pokebeach.com/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'flog pl', 'tags': u'daily', 'href': u'http://flog.pl/', 'user': u'', 'dt': ''}, {'count': '', 'extended': u'', 'hash': '', 'description': u'anwsion com', 'tags': u'write', 'href': u'http://anwsion.com/', 'user': u'', 'dt': ''}]

返回了一个包含字典的列表，每一项都包含了：URL，描述，提交者。

pydelicous里还有其他函数，比如get_urlposts：返回指定URL的所有张贴记录

get_userposts：返回给定用户的所有张贴记录

OK,下边我们构建一个基于del.icio.us的链接推荐系统

1.首先，我们需要利用pydelicious.py构造一个数据集，里边有多个用户，每个用户标记了其热爱的链接。

<pre class="lang:default decode:true">from pydelicious import get_popular,get_userposts,get_urlposts
import time

def initializeUserDict(tag,count=5):
  user_dict={}
  # get the top count' popular posts
  for p1 in get_popular(tag=tag)[0:count]:
    # find all users who posted this
    for p2 in get_urlposts(p1['href']):
      user=p2['user']
      user_dict[user]={}
  return user_dict

def fillItems(user_dict):
  all_items={}
  # Find links posted by all users
  for user in user_dict:
    for i in range(3):
      try:
        posts=get_userposts(user)
        break
      except:
        print "Failed user "+user+", retrying"
        time.sleep(4)
    for post in posts:
      url=post['href']
      user_dict[user][url]=1.0
      all_items[url]=1

  # Fill in missing items with 0
  for ratings in user_dict.values():
    for item in all_items:
      if item not in ratings:
        ratings[item]=0.0</pre>

保存为deliciousrec.py

运行：>>>from deliciousrec import *

>>>delusers=initializeUserDict(&#8216;music&#8217;)

>>>delusers['Lazynight']={}

>>>fillItems(delusers)

这样我们把获取的数据集保存到了delusers中

注意：获取数据集需要等待1分钟左右，量比较大，如果失败请重新尝试几次。

2.下边我们推荐给用户邻近的链接(下边的r为上节写过的r.py)

>>> import random  
>>> user=delusers.keys()[random.randint(0,len(delusers)-1)]  
>>> user  
u&#8217;nafosyxog&#8217;  
>>> r.topMatches(delusers,user)  
[(0.14482758620689656, u'letorocoqet'), (0.03793103448275862, u'lydonahej'), (-0.042742263849460935, 'Lazynight'), (-0.0526985286556622, u'tiqifojeg'), (-0.0526985286556622, u'syfasezeve')]  
>>> r.getRecommendations(delusers,user)[0:10]  
[(1.0, u'http://asus.com.cn/'), (0.7924528301886793, u'http://voici-news.fr/'), (0.7924528301886793, u'http://sheinside.com/'), (0.7924528301886793, u'http://setlktv.info/'), (0.7924528301886793, u'http://rpi.edu/'), (0.7924528301886793, u'http://imoneynews.com/'), (0.7924528301886793, u'http://desibbrg.com/'), (0.7924528301886793, u'http://cholotube.com/'), (0.20754716981132074, u'http://shahiya.com/'), (0.20754716981132074, u'http://pool.com/')]  
3.至此我们为delicious添加了一个推荐引擎~

下节说说基于物品的过滤~

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [构建一个基于del.icio.us的链接推荐系统][3]

 [1]: delicious.com
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2740.html