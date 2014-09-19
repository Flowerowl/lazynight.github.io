---
title: 'jQuery 抄袭笔记(14) : Parent()(2)'
author: Flowerowl
layout: post
permalink: /527.html
duoshuo_thread_id:
  - 1266854
views:
  - 808
bot_views:
  - 2
categories:
  - jQuery
  - 代码
---
<pre>this.parent()是input前面的td
this.parent().parent()获取的是tr
this.parent().parent().parent()获取的是table
this.parent().next()获取的是td相临的td</pre>

<img class="aligncenter size-full wp-image-529" title="Lazynight | 夜阑" src="http://lazynight.me/wp-content/uploads/2011/10/111.jpg" alt="" width="384" height="307" />

&nbsp;

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #696969;">01</span> <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #696969;">02</span> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">03</span> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #696969;">04</span> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #f810b0;">05</span> <span style="color: #ff4400; font-weight: bold;"><title></span>Hello Lazynight!<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #696969;">06</span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;http://www.cssrain.cn/demo/jquery.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #696969;">07</span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">08</span> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #696969;">09</span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;#Lazy_btn&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">click</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #f810b0;">10</span> <span style="color: #c0c0c0;">alert</span>(<span style="color: #c0c0c0;">$</span>(<span style="color: #ff4400; font-weight: bold;">this</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">parent</span><span style="color: #c0c0c0;">().</span><span style="color: #c0c0c0;">next</span><span style="color: #c0c0c0;">().</span><span style="color: #c0c0c0;">html</span>());<br /> <span style="color: #696969;">11</span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #696969;">12</span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #696969;">13</span> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #696969;">14</span> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #f810b0;">15</span> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #696969;">16</span> <span style="color: #ff4400; font-weight: bold;"><table></span><br /> <span style="color: #696969;">17</span> <span style="color: #ff4400; font-weight: bold;"><tr></span><br /> <span style="color: #696969;">18</span>     <span style="color: #ff4400; font-weight: bold;"><td><input</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;Lazy_btn&#8221;</span> <span style="color: #ffff00;">class=</span><span style="color: #d13800;">&#8220;btn&#8221;</span> <span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;button&#8221;</span> <span style="color: #ffff00;">value=</span><span style="color: #d13800;">&#8220;text&#8221;</span><span style="color: #ff4400; font-weight: bold;">/></td></span><br /> <span style="color: #696969;">19</span>     <span style="color: #ff4400; font-weight: bold;"><td></span>Lazynight | 夜阑 欢迎您！<span style="color: #ff4400; font-weight: bold;"></td></span><br /> <span style="color: #f810b0;">20</span><br /> <span style="color: #696969;">21</span>     <span style="color: #ff4400; font-weight: bold;"><pre></span><br /> <span style="color: #696969;">22</span>     this.parent()是input前面的td<br /> <span style="color: #696969;">23</span>     this.parent().parent()获取的是tr<br /> <span style="color: #696969;">24</span>     this.parent().parent().parent()获取的是table<br /> <span style="color: #f810b0;">25</span>     this.parent().next()获取的是td相临的td<br /> <span style="color: #696969;">26</span>     <span style="color: #ff4400; font-weight: bold;"></pre></span><br /> <span style="color: #696969;">27</span> <span style="color: #ff4400; font-weight: bold;"></tr></span><br /> <span style="color: #696969;">28</span> <span style="color: #ff4400; font-weight: bold;"></table></span><br /> <span style="color: #696969;">29</span> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #f810b0;">30</span> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff6600;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy14_Parent()(2).rar/.page" target="_blank"><span style="color: #ff6600;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(14) : Parent()(2)][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/527.html