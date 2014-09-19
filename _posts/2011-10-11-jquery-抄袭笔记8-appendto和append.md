---
title: 'jQuery 抄袭笔记(8) : Appendto和Append'
author: Flowerowl
layout: post
permalink: /439.html
duoshuo_thread_id:
  - 1266841
views:
  - 682
bot_views:
  - 3
categories:
  - jQuery
  - 代码
---
  
通过append或者appendto方法可以向元素之后添加内容。

两者区别是：

<pre>1.</pre>

<pre>$(<em>content</em>).appendTo(<em>selector</em>)</pre>

<pre>2.</pre>

<pre>$(selector).append(<em>content</em>)</pre>

<pre><span style="color: #ff0000;">选择器和内容位置颠倒。</span></pre>

<pre><span style="color: #ff0000;">append() 能够使用函数来附加内容。</span></pre>

<img class="aligncenter size-full wp-image-440" title="Lazynight | 夜阑" src="http://lazynight.me/wp-content/uploads/2011/10/0111011201311.jpg" alt="" width="316" height="361" />

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #696969;">01 </span> <span style="color: #ffffff;"><!DOCTYPE html PUBLIC &#8220;-//W3C//DTD XHTML 1.0 Transitional//EN&#8221; &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span><br /> <span style="color: #696969;">02 </span> <span style="color: #ff4400; font-weight: bold;"><html</span> <span style="color: #ffff00;">xmlns=</span><span style="color: #d13800;">&#8220;http://www.w3.org/1999/xhtml&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">03 </span> <span style="color: #ff4400; font-weight: bold;"><head></span><br /> <span style="color: #696969;">04 </span> <span style="color: #ff4400; font-weight: bold;"><meta</span> <span style="color: #ffff00;">http-equiv=</span><span style="color: #d13800;">&#8220;Content-Type&#8221;</span> <span style="color: #ffff00;">content=</span><span style="color: #d13800;">&#8220;text/html; charset=gb2312&#8243;</span> <span style="color: #ff4400; font-weight: bold;">/></span><br /> <span style="color: #f810b0;">05 </span> <span style="color: #ff4400; font-weight: bold;"><title></span>AppendTo的用法<span style="color: #ff4400; font-weight: bold;"></title></span><br /> <span style="color: #696969;">06 </span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span> <span style="color: #ffff00;">src=</span><span style="color: #d13800;">&#8220;jquery-1.1.3.pack.js&#8221;</span><span style="color: #ff4400; font-weight: bold;">></script></span><br /> <span style="color: #696969;">07 </span> <span style="color: #ff4400; font-weight: bold;"><script </span><span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;text/javascript&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span><br /> <span style="color: #696969;">08 </span> <span style="color: #c0c0c0;">$</span>(<span style="color: #c0c0c0;">document</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">ready</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #696969;">09 </span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;#Lazy_btn1&#8243;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">click</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #f810b0;">10 </span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;<input type=&#8217;text&#8217; name=&#8217;text1&#8242; id=&#8217;text1&#8242; value=&#8217;Lazynight&#8217; ><br>&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">appendTo</span>(<span style="color: #d13800;">&#8220;#night&#8221;</span>);<br /> <span style="color: #696969;">11 </span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #696969;">12 </span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;#Lazy_btn2&#8243;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">click</span>(<span style="color: #ff4400; font-weight: bold;">function</span><span style="color: #c0c0c0;">(){</span><br /> <span style="color: #696969;">13 </span> <span style="color: #c0c0c0;">$</span>(<span style="color: #d13800;">&#8220;#day&#8221;</span><span style="color: #c0c0c0;">).</span><span style="color: #c0c0c0;">append</span>(<span style="color: #d13800;">&#8220;<input type=&#8217;text&#8217; name=&#8217;text2&#8242; id=&#8217;text2&#8242; value=&#8217;Lazynight&#8217; ><br>&#8221;</span>);<br /> <span style="color: #696969;">14 </span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #f810b0;">15 </span> <span style="color: #c0c0c0;">});</span><br /> <span style="color: #696969;">16 </span> <span style="color: #ff4400; font-weight: bold;"></script></span><br /> <span style="color: #696969;">17 </span> <span style="color: #ff4400; font-weight: bold;"></head></span><br /> <span style="color: #696969;">18 </span> <span style="color: #ff4400; font-weight: bold;"><body></span><br /> <span style="color: #696969;">19 </span> <span style="color: #ff4400; font-weight: bold;"><input</span> <span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;button&#8221;</span> <span style="color: #ffff00;">name=</span><span style="color: #d13800;">&#8220;btn1&#8243;</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;Lazy_btn1&#8243;</span> <span style="color: #ffff00;">value=</span><span style="color: #d13800;">&#8220;appendto&#8221;</span><span style="color: #ff4400; font-weight: bold;">><br/></span><br /> <span style="color: #f810b0;">20 </span> <span style="color: #ff4400; font-weight: bold;"><input</span> <span style="color: #ffff00;">type=</span><span style="color: #d13800;">&#8220;button&#8221;</span> <span style="color: #ffff00;">name=</span><span style="color: #d13800;">&#8220;btn2&#8243;</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;Lazy_btn2&#8243;</span> <span style="color: #ffff00;">value=</span><span style="color: #d13800;">&#8220;append&#8221;</span><span style="color: #ff4400; font-weight: bold;">><br/></span><br /> <span style="color: #696969;">21 </span> <span style="color: #ff4400; font-weight: bold;"><div</span> <span style="color: #ffff00;">name=</span><span style="color: #d13800;">&#8220;night&#8221;</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;night&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span>AppendTo组的变化<span style="color: #ff4400; font-weight: bold;"></div><br/></span><br /> <span style="color: #696969;">22 </span> <span style="color: #ff4400; font-weight: bold;"><div</span> <span style="color: #ffff00;">name=</span><span style="color: #d13800;">&#8220;day&#8221;</span> <span style="color: #ffff00;">id=</span><span style="color: #d13800;">&#8220;day&#8221;</span><span style="color: #ff4400; font-weight: bold;">></span>Append组的变化<span style="color: #ff4400; font-weight: bold;"></div><br/></span><br /> <span style="color: #696969;">23 </span> <span style="color: #ff4400; font-weight: bold;"></body></span><br /> <span style="color: #696969;">24 </span> <span style="color: #ff4400; font-weight: bold;"></html></span>
</div>

<span style="color: #ff0000;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy8_AppendTo-7684-7528-6cd5.rar/.page" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(8) : Appendto和Append][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/439.html