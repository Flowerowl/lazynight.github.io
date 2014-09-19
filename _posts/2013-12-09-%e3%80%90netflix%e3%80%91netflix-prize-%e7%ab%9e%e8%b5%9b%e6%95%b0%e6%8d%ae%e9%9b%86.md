---
title: 【Netflix】Netflix Prize 竞赛数据集
author: Flowerowl
layout: post
permalink: /3294.html
views:
  - 1025
duoshuo_thread_id:
  - 1220743779864322545
categories:
  - 资源
tags:
  - data
  - netflix
  - netflixprize
  - 数据中心
  - 竞赛
---
**netflix不提供数据下载了，从网上down的一份，分享之。**

<img title="NewImage.png" src="http://lazynight.me/wp-content/uploads/2013/12/NewImage1.png" alt="NewImage" width="194" height="161" border="0" />

**下载：<http://pan.baidu.com/s/1hAdpU>**

**相关：<span style="line-height: 28px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;"><span style="font-family: 楷体_GB2312;">DVD在线租赁商 </span></span><a style="line-height: 21px; color: #d46173; font-size: 12px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;" href="http://www.netflix.com/"><span style="line-height: 28px; font-size: medium;"><span style="font-family: 楷体_GB2312;">Netflix</span></span></a><span style="line-height: 28px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;"><span style="font-family: 楷体_GB2312;"> 于 2006 年 10 月 2 日发起一项竞赛：</span></span><a style="line-height: 21px; color: #d46173; font-size: 12px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;" href="http://www.netflixprize.com/"><span style="line-height: 28px; font-size: medium;"><span style="font-family: 楷体_GB2312;">Netflix Prize</span></span></a><span style="line-height: 28px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;"><span style="font-family: 楷体_GB2312;">，任何组织或个人 只要能够提交比它现有电影推荐系统 Cinematch 效果好 10% 的新方法，就可以获得一百万美元的奖金。竞赛最多持续到 2011 年 10 月 2 日。同时，</span></span><a style="line-height: 21px; color: #d46173; font-size: 12px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;" href="http://www.netflixprize.com/"><span style="line-height: 28px; font-size: medium;"><span style="font-family: 楷体_GB2312;">Netflix Prize</span></span></a><span style="line-height: 28px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;"><span style="font-family: 楷体_GB2312;"> 还提供每年五万美元的年度进步奖。2007 年年度进步奖由来自 AT&T 的 </span></span><a style="line-height: 21px; color: #d46173; font-size: 12px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;" href="http://www.research.att.com/~volinsky/netflix/"><span style="line-height: 28px; font-size: medium;"><span style="font-family: 楷体_GB2312;">BellKor</span></span></a><span style="line-height: 28px; font-family: Verdana, Arial, Helvetica, 宋体, sans-serif;"><span style="font-family: 楷体_GB2312;"> 小组夺得。</span></span>**

**官网：<http://www.netflixprize.com/>**

**  
**

**README:**

SUMMARY  
================================================================================

This dataset was constructed to support participants in the Netflix Prize. See  
http://www.netflixprize.com for details about the prize.

The movie rating files contain over 100 million ratings from 480 thousand  
randomly-chosen, anonymous Netflix customers over 17 thousand movie titles. The  
data were collected between October, 1998 and December, 2005 and reflect the  
distribution of all ratings received during this period. The ratings are on a  
scale from 1 to 5 (integral) stars. To protect customer privacy, each customer  
id has been replaced with a randomly-assigned id. The date of each rating and  
the title and year of release for each movie id are also provided.

USAGE LICENSE  
================================================================================

Netflix can not guarantee the correctness of the data, its suitability for any  
particular purpose, or the validity of results based on the use of the data set.  
The data set may be used for any research purposes under the following  
conditions:

* The user may not state or imply any endorsement from Netflix.

* The user must acknowledge the use of the data set in  
publications resulting from the use of the data set, and must  
send us an electronic or paper copy of those publications.

* The user may not redistribute the data without separate  
permission.

* The user may not use this information for any commercial or  
revenue-bearing purposes without first obtaining permission  
from Netflix.

If you have any further questions or comments, please contact the Prize  
administrator <prizemaster@netflix.com>

TRAINING DATASET FILE DESCRIPTION  
================================================================================

The file &#8220;training_set.tar&#8221; is a tar of a directory containing 17770 files, one  
per movie. The first line of each file contains the movie id followed by a  
colon. Each subsequent line in the file corresponds to a rating from a customer  
and its date in the following format:

CustomerID,Rating,Date

- MovieIDs range from 1 to 17770 sequentially.  
- CustomerIDs range from 1 to 2649429, with gaps. There are 480189 users.  
- Ratings are on a five star (integral) scale from 1 to 5.  
- Dates have the format YYYY-MM-DD.

MOVIES FILE DESCRIPTION  
================================================================================

Movie information in &#8220;movie_titles.txt&#8221; is in the following format:

MovieID,YearOfRelease,Title

- MovieID do not correspond to actual Netflix movie ids or IMDB movie ids.  
- YearOfRelease can range from 1890 to 2005 and may correspond to the release of  
corresponding DVD, not necessarily its theaterical release.  
- Title is the Netflix movie title and may not correspond to   
titles used on other sites. Titles are in English.

QUALIFYING AND PREDICTION DATASET FILE DESCRIPTION  
================================================================================

The qualifying dataset for the Netflix Prize is contained in the text file  
&#8220;qualifying.txt&#8221;. It consists of lines indicating a movie id, followed by a  
colon, and then customer ids and rating dates, one per line for that movie id.  
The movie and customer ids are contained in the training set. Of course the  
ratings are withheld. There are no empty lines in the file.

MovieID1:  
CustomerID11,Date11  
CustomerID12,Date12  
&#8230;  
MovieID2:  
CustomerID21,Date21  
CustomerID22,Date22

For the Netflix Prize, your program must predict the all ratings the customers  
gave the movies in the qualifying dataset based on the information in the  
training dataset.

The format of your submitted prediction file follows the movie and customer id,  
date order of the qualifying dataset. However, your predicted rating takes the  
place of the corresponding customer id (and date), one per line.

For example, if the qualifying dataset looked like:

111:  
3245,2005-12-19  
5666,2005-12-23  
6789,2005-03-14  
225:  
1234,2005-05-26  
3456,2005-11-07

then a prediction file should look something like:  
111:  
3.0  
3.4  
4.0  
225:  
1.0  
2.0

which predicts that customer 3245 would have rated movie 111 3.0 stars on the  
19th of Decemeber, 2005, that customer 5666 would have rated it slightly higher  
at 3.4 stars on the 23rd of Decemeber, 2005, etc.

You must make predictions for all customers for all movies in the qualifying  
dataset.

THE PROBE DATASET FILE DESCRIPTION  
================================================================================

To allow you to test your system before you submit a prediction set based on the  
qualifying dataset, we have provided a probe dataset in the file &#8220;probe.txt&#8221;.  
This text file contains lines indicating a movie id, followed by a colon, and  
then customer ids, one per line for that movie id.

MovieID1:  
CustomerID11  
CustomerID12  
&#8230;  
MovieID2:  
CustomerID21  
CustomerID22

Like the qualifying dataset, the movie and customer id pairs are contained in  
the training set. However, unlike the qualifying dataset, the ratings (and  
dates) for each pair are contained in the training dataset.

If you wish, you may calculate the RMSE of your predictions against those  
ratings and compare your RMSE against the Cinematch RMSE on the same data. See  
http://www.netflixprize.com/faq#probe for that value.

Good luck!

MD5 SIGNATURES AND FILE SIZES  
================================================================================

d2b86d3d9ba8b491d62a85c9cf6aea39 577547 movie_titles.txt  
ed843ae92adbc70db64edbf825024514 10782692 probe.txt  
88be8340ad7b3c31dfd7b6f87e7b9022 52452386 qualifying.txt  
0e13d39f97b93e2534104afc3408c68c 567 rmse.pl  
0098ee8997ffda361a59bc0dd1bdad8b 2081556480 training_set.tar

 

 ====================================分割线======================================

**以下是个人对数据集格式的理解**

<!--?xml version="1.0" encoding="UTF-8" standalone="no"?-->

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;">统计</span>
</div>

<span style="font-family: Arial;">1亿打分</span>

<div style="font-family: Arial;">
  <span style="white-space: pre-wrap;">480189</span>用户
</div>

<div style="font-family: Arial;">
  17770电影
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;">格式：</span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;">training_set </span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
  MovieID：
</div>

<div style="font-family: Arial;">
  <span style="white-space: pre-wrap;">CustomerID,Rating,Date</span>
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  mv_0000001.txt 
</div>

<span style="font-family: Arial;">1:</span>

<div style="font-family: Arial;">
  1488844,3,2005-09-06
</div>

<span style="font-family: Arial;">822109,5,2005-05-13</span>

<div style="font-family: Arial;">
  885013,4,2005-10-19
</div>

<div style="font-family: Arial;">
  30878,4,2005-12-26
</div>

<div style="font-family: Arial;">
  823519,3,2004-05-03
</div>

<div style="font-family: Arial;">
  893988,3,2005-11-17
</div>

<div style="font-family: Arial;">
  124105,4,2004-08-05
</div>

<div style="font-family: Arial;">
  1248029,3,2004-04-22
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  mv_0000002.txt
</div>

<span style="font-family: Arial;">2:</span>

<div style="font-family: Arial;">
  2059652,4,2005-09-05
</div>

<div style="font-family: Arial;">
  1666394,3,2005-04-19
</div>

<div style="font-family: Arial;">
  1759415,4,2005-04-22
</div>

<div style="font-family: Arial;">
  1959936,5,2005-11-21
</div>

<div style="font-family: Arial;">
  998862,4,2004-11-13
</div>

<div style="font-family: Arial;">
  2625420,2,2004-12-06
</div>

<div style="font-family: Arial;">
  573975,3,2005-07-21  
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5; white-space: pre-wrap;">movie_titles.txt</span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5; white-space: pre-wrap;"><br /></span>
</div>

<div style="font-family: Arial;">
  <span style="white-space: pre-wrap;">MovieID,YearOfRelease,Title</span><span style="background-color: #fffaa5; white-space: pre-wrap;"><br /></span>
</div>

<div style="font-family: Arial;">
   
</div>

<span style="font-family: Arial;">1,2003,Dinosaur Planet</span>

<div style="font-family: Arial;">
  2,2004,Isle of Man TT 2004 Review
</div>

<div style="font-family: Arial;">
  3,1997,Character
</div>

<div style="font-family: Arial;">
  4,1994,Paula Abdul&#8217;s Get Up & Dance
</div>

<div style="font-family: Arial;">
  5,2004,The Rise and Fall of ECW
</div>

<div style="font-family: Arial;">
  6,1997,Sick
</div>

<div style="font-family: Arial;">
  7,1992,8 Man
</div>

<div style="font-family: Arial;">
  8,2004,What the #$*! Do We Know!?
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;"> qualifying.txt</span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
  参赛团队提交整个Qualifying Set上的预测评分值 <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
   
</div>

<span style="font-family: Arial;">MovieID1:</span>

<div style="font-family: Arial;">
  CustomerID11,Date11
</div>

<div style="font-family: Arial;">
  CustomerID12,Date12
</div>

<div style="font-family: Arial;">
  &#8230;
</div>

<div style="font-family: Arial;">
  MovieID2:
</div>

<div style="font-family: Arial;">
  CustomerID21,Date21
</div>

<div style="font-family: Arial;">
  CustomerID22,Date22
</div>

<div style="font-family: Arial;">
   <span style="background-color: #fffaa5;"><br /></span>
</div>

<span style="font-family: Arial;">1:</span>

<div style="font-family: Arial;">
  1046323,2005-12-19
</div>

<div style="font-family: Arial;">
  1080030,2005-12-23
</div>

<div style="font-family: Arial;">
  1830096,2005-03-14
</div>

<div style="font-family: Arial;">
  368059,2005-05-26
</div>

<div style="font-family: Arial;">
  802003,2005-11-07
</div>

<div style="font-family: Arial;">
  513509,2005-07-04 <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
   
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;">probe.txt</span>
</div>

<div style="font-family: Arial;">
  <span style="background-color: #fffaa5;"><br /></span>
</div>

<div style="font-family: Arial;">
  用来对照检测你对qualifying.txt预测的结果
</div>

<div style="font-family: Arial;">
   
</div>

<span style="font-family: Arial;">MovieID1:</span>

<div style="font-family: Arial;">
  CustomerID11
</div>

<div style="font-family: Arial;">
  CustomerID12
</div>

<div style="font-family: Arial;">
  &#8230;
</div>

<div style="font-family: Arial;">
  MovieID2:
</div>

<div style="font-family: Arial;">
  CustomerID21
</div>

<div style="font-family: Arial;">
  CustomerID22
</div>

<div style="font-family: Arial;">
   <span style="background-color: #fffaa5;"><br /></span>
</div>

<span style="font-family: Arial;">1:</span>

<div style="font-family: Arial;">
  30878
</div>

<div style="font-family: Arial;">
  2647871
</div>

<div style="font-family: Arial;">
  1283744
</div>

<div style="font-family: Arial;">
  2488120
</div>

<div style="font-family: Arial;">
  317050
</div>

<div style="font-family: Arial;">
  1904905
</div>

<div style="font-family: Arial;">
  1989766
</div>

<div style="font-family: Arial;">
  14756  
</div>

**  
**

转载请注明：[于哲的博客][1] &raquo; [【Netflix】Netflix Prize 竞赛数据集][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3294.html