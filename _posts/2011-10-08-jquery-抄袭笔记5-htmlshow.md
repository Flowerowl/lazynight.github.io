---
title: 'jQuery 抄袭笔记(5) : Html(),Show()'
author: Flowerowl
layout: post
permalink: /405.html
duoshuo_thread_id:
  - 1266834
views:
  - 1131
bot_views:
  - 3
categories:
  - jQuery
  - 代码
---
1. 无参html（）：取得第一个匹配元素的html内容。这个函数不能用于XML文档。但可以用于XHTML文档，返回的是一个String

html页面代码：<div><p>Hello</p></div>

jquery代码：$(&#8220;div&#8221;).html();

结果：Hello  
2.有参html（val）：设置每一个匹配元素的html内容。这个函数不能用于XML文档。但可以用于XHTML文档。返回一个jquery对象

html页面代码：<div></div>

jquery代码：$(&#8220;div&#8221;).html(&#8220;<p>Nice to meet you</p>&#8221;);

结果：[ <div><p> Nice to meet you</p></div> ]

3.show():该效果适用于通过 jQuery 隐藏的元素，或在 CSS 中声明 display:none 的元素（但不适用于 visibility:hidden 的元素）。

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #ff4400; font-weight: bold;"><title></span>hello world<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;http://www.cssrain.cn/demo/jquery.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #696969;">//$(&#8220;a&#8221;).addClass(&#8220;test&#8221;).show().html(&#8220;Lazynight&#8221;);</span><br /> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;a&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">addClass</span>(<span style="color: #d13800;">&#8220;test&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">html</span>(<span style="color: #d13800;">&#8220;Lazynight&#8221;</span>);<br /> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #ff4400; font-weight: bold;"><style></span><br /> <span style="color: #c0c0c0;">.test</span><span style="color: #c0c0c0;">{</span><span style="color: #ff4400; font-weight: bold;">color</span><span style="color: #c0c0c0;">:</span><span style="color: #c0c0c0;">red</span><span style="color: #c0c0c0;">;}</span><br /> <span style="color: #ff4400; font-weight: bold;"></style></span><br /> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #ff4400; font-weight: bold;"><a</span> <span style="color: #ffff00;">href=</span><span style="color: #d13800;">&#8220;http://lazynight.me&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> 夜阑<br /> <span style="color: #ff4400; font-weight: bold;"></a></span><br /> <span style="color: #ff4400; font-weight: bold;"><br></span><br /> <span style="color: #ff4400; font-weight: bold;"><a</span> <span style="color: #ffff00;">href=</span><span style="color: #d13800;">&#8220;http://lazynight.me&#8221;</span> <span style="color: #ffff00;">style=</span><span style="color: #d13800;">&#8220;display:none;&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> 夜阑<br /> <span style="color: #ff4400; font-weight: bold;"></a></span><br /> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff0000;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy5-002dshow(_)-548chtml().rar/.page" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(5) : Html(),Show()][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/405.html