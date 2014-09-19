---
title: 'jQuery 抄袭笔记(6) : Hide特效'
author: Flowerowl
layout: post
permalink: /420.html
duoshuo_thread_id:
  - 1266837
views:
  - 779
bot_views:
  - 1
categories:
  - jQuery
  - 代码
---
  
摘自w3school ：

$(this).hide() &#8211; 隐藏当前元素

$(&#8220;p&#8221;).hide() &#8211; 隐藏所有段落

$(&#8220;p.test&#8221;).hide() &#8211; 隐藏所有 的段落

$(&#8220;#test&#8221;).hide() &#8211; 隐藏所有 id=&#8221;test&#8221; 的元素

hide() 和 show() 都可以设置两个可选参数：speed 和 callback。

speed 参数可以设置这些值：&#8221;slow&#8221;, &#8220;fast&#8221;, &#8220;normal&#8221; 或 milliseconds

callback 参数是在 hide 或 show 函数完成之后被执行的函数名称。

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #ff4400; font-weight: bold;"><title></span>Hello Lazynight!<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;jquery-1.1.3.pack.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;a&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">click</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #ff4400; font-weight: bold;">this</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">hide</span>(<span style="color: #d13800;">&#8220;slow&#8221;</span>);<br /> <span style="color: #ff4400; font-weight: bold;">return</span> <span style="color: #ff4400; font-weight: bold;">false</span>;<br /> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #ff4400; font-weight: bold;"><a</span> <span style="color: #ffff00;">href=</span><span style="color: #d13800;">&#8220;http://lazynight.me&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> 点我吧~点我我就慢慢消失&#8230;<br /> “return false”表示页面不会跳转至链接页面<br /> <span style="color: #ff4400; font-weight: bold;"></a></span><br /> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff0000;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy6_hide-7279-6548.rar/.page" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(6) : Hide特效][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/420.html