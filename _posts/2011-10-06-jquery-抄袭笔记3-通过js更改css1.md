---
title: 'jQuery 抄袭笔记(3) : 通过JS更改CSS(1)'
author: Flowerowl
layout: post
permalink: /373.html
duoshuo_thread_id:
  - 1266830
views:
  - 708
bot_views:
  - 2
categories:
  - jQuery
  - 代码
---
  
好多的站都有一个&#8221;隐藏/显示侧边栏&#8221;或者改变主题颜色的按钮，其实很好实现滴~  
这节很简单，通过JS增加一个class，然后用css改变字体颜色。  
<img class="aligncenter size-full wp-image-378" title="Lazynight | 夜阑" src="http://lazynight.me/wp-content/uploads/2011/10/20111007071240.jpg" alt="" width="351" height="73" />  
Html+JS+CSS:

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #ff4400; font-weight: bold;"><title></span>Hello Lazynight!<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;jquery-1.1.3.pack.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;#css1&#8243;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">addClass</span>(<span style="color: #d13800;">&#8220;css2&#8243;</span>);<br /> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #ff4400; font-weight: bold;"><style></span><br /> <span style="color: #c0c0c0;">.css2</span><span style="color: #c0c0c0;">{</span><span style="color: #ff4400; font-weight: bold;">color</span><span style="color: #c0c0c0;">:</span><span style="color: #c0c0c0;">red</span><span style="color: #c0c0c0;">;}</span><br /> <span style="color: #ff4400; font-weight: bold;"></style></span><br /> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #ff4400; font-weight: bold;"><div</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;css1&#8243;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> 通过JS添加一个class“css2”，就是这么简单！<br /> <span style="color: #ff4400; font-weight: bold;"></div></span><br /> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff0000;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy3_-901a-8fc7JS-6539-53d8CSS(1).rar/.page" target="_blank"><span style="color: #ff0000;"> 下载源码</span></a></span>

<span style="color: #ff0000;">解压密码</span>   <span style="color: #ffffff;"><strong>  lazynight.me</strong></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(3) : 通过JS更改CSS(1)][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/373.html