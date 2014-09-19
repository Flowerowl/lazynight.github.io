---
title: 'jQuery 抄袭笔记(9) : Toggle()'
author: Flowerowl
layout: post
permalink: /461.html
duoshuo_thread_id:
  - 1266844
views:
  - 574
bot_views:
  - 2
categories:
  - jQuery
  - 代码
---
<embed src="http://www.xiami.com/widget/0_3556769/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent">
</embed>

  
&nbsp;

摘自w3school：

toggle() 方法切换元素的可见状态。

如果被选元素可见，则隐藏这些元素，如果被选元素隐藏，则显示这些元素。

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #696969;">01</span> <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #696969;">02</span> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">03</span> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #696969;">04</span> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #f810b0;">05</span> <span style="color: #ff4400; font-weight: bold;"><title></span>toggle的用法<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #696969;">06</span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;jquery-1.1.3.pack.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #696969;">07</span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">08</span> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #696969;">09</span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;p&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">toggle</span>()<br /> <span style="color: #f810b0;">10</span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #696969;">11</span> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #696969;">12</span> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #696969;">13</span> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #696969;">14</span> <span style="color: #ff4400; font-weight: bold;"><p></span>Lazynight 这里本来是显示的哦！但是现在通过toggle（）它隐藏了！<span style="color: #ff4400; font-weight: bold;"></p></span><br /> <span style="color: #f810b0;">15</span> <span style="color: #ff4400; font-weight: bold;"><p</span> <span style="color: #ffff00;">style=</span><span style="color: #d13800;">&#8220;display: none&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span>Lazynight 这里本来是隐藏的哦！但是现在通过toggle（）它显示了！<span style="color: #ff4400; font-weight: bold;"></p></span><br /> <span style="color: #696969;">16</span> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #696969;">17</span> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0mcag311t" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(9) : Toggle()][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/461.html