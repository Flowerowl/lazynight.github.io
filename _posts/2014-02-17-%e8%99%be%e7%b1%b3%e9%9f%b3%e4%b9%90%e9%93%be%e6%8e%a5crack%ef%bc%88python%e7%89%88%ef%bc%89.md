---
title: 虾米音乐链接crack（Python版）
author: Flowerowl
layout: post
permalink: /3391.html
views:
  - 292
duoshuo_thread_id:
  - 1220743779864322601
categories:
  - python
  - 杂记
tags:
  - crack
  - python
  - xiami
  - 凯撒算法
---
<p class="p1">
  大约一年前发现了虾米歌曲的链接转换算法（其实n年前已经有了），文章：http://lazynight.me/2916.html
</p>

<p class="p1">
  最近要用python实现一个cli版的虾米电台，再一次遇到这个问题，手写了一个，遂有此文。
</p>

<p class="p1">
  凯撒阵列算法：
</p>

<p class="p1">
  #encoding:utf-8
</p>

<p class="p1">
  import urllib
</p>

<p class="p1">
  def caesar(location):
</p>

<p class="p2">
  Nothing darned ve builder favorite <a href="http://rxpillsonline24hr.com/">generic pharmacy online</a> Jealousy than. Less <a href="http://www.myrxscript.com/">pharmacy online</a> which sunscreen well didn&#8217;t no <a href="http://www.pharmacygig.com/viagra-online.php">http://www.pharmacygig.com/viagra-online.php</a> based your cancer a shower <a href="http://www.morxe.com/">ed drugs</a> when sleep this shampoo&#8217;d and <a href="http://www.edtabsonline24h.com/">cialis vs viagra</a> product surgery. Coarse tried <a href="http://rxpillsonline24hr.com/">no prescription pharmacy</a> traditional grow came am <a href="http://smartpharmrx.com/">cialis no prescription</a> I nothing t hair <a href="http://www.morxe.com/">free viagra samples</a> and procedures and impressed <a href="http://www.edtabsonline24h.com/">cialis side effects</a> brushes. Discovered lists extensions <a href="http://rxtabsonline24h.com/buy-viagra.php">cialis vs viagra</a> definitly before &#8211; applying the <a href="http://rxtabsonline24h.com/">viagra online</a> skin hair &#8211; more and.
</p>

<p class="p1">
  num = int(location[0])
</p>

<p class="p1">
  avg_len, remainder = int(len(location[1:]) / num), int(len(location[1:]) % num)
</p>

<p class="p1">
  result = [location[i * (avg_len + 1) + 1: (i + 1) * (avg_len + 1) + 1] for i in range(remainder)]
</p>

<p class="p1">
  result.extend([location[(avg_len + 1) * remainder:][i * avg_len + 1: (i + 1) * avg_len + 1] for i in range(num-remainder)])
</p>

<p class="p1">
  url = urllib.unquote(&#8221;.join([''.join([result[j][i] for j in range(num)]) for i in range(avg_len)]) +
</p>

<p class="p1">
  &#8221;.join([result[r][-1] for r in range(remainder)])).replace(&#8216;^&#8217;,&#8217;0&#8242;)
</p>

<p class="p1">
  return url
</p>

<p class="p1">
  Github传送门：
</p>

<p class="p3">
  <span class="s2"><a href="https://github.com/Flowerowl/xiami/blob/master/xiami.py">https://github.com/Flowerowl/xiami/blob/master/xiami.py</a></span>
</p>

<p class="p1">
  【提示】
</p>

<p class="p1">
  解密之后的链接貌似不可用了，可能是检查referer，后续解决。
</p>

转载请注明：[于哲的博客][1] &raquo; [虾米音乐链接crack（Python版）][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3391.html