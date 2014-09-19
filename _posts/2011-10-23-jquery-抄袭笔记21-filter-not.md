---
title: jQuery 抄袭笔记(21) :filter()，not()
author: Flowerowl
layout: post
permalink: /632.html
duoshuo_thread_id:
  - 1266874
views:
  - 756
bot_views:
  - 6
categories:
  - jQuery
  - 代码
---
<span style="color: #ff6600;">本想贴John McDaid的Stay There。结果虾米没有，不过这首也是大爱~尤其是最后反反复复高亢又低沉的声音再一次让我欲罢不能 。</span>

* * *

<span style="color: #ff6600;">not()</span> 从匹配元素集合中删除元素。

如果给定一个表示 DOM 元素集合的 jQuery 对象，.not() 方法会用匹配元素的子集构造一个新的 jQuery 对象。所应用的选择器会检测每个元素；不匹配该选择器的元素会被包含在结果中。  
<span style="color: #ff6600;">filter()</span> 方法将匹配元素集合缩减为匹配指定选择器的元素。  
如果给定表示 DOM 元素集合的 jQuery 对象，.filter() 方法会用匹配元素的子集构造一个新的 jQuery 对象。所使用的选择器会测试每个元素；所有匹配该选择器的元素都会包含在结果中。

<span style="color: #ff6600;"> filter()与find()的区别</span>

find()会在div元素内 寻找 class为classname的元素。

filter()则是筛选div的class为classname的元素。

基本是find子元素找，filter是平级找

·find 函数是在当前对象集合的子元素中进行查询;  
·filter 函数是对当前对象集合进行过滤, 利用过滤条件缩小范围;  
·find 函数的参数是 jQuery 选择器表达式;  
·filter 的参数也是选择器表达式, 但可以有多个条件, 用逗号分隔(逻辑或关系);  
·filter 的参数也可以是个函数, 调用函数时会自动传入 index 参数, 函数需返回 true或false 以选中或排除元素.

<img class="aligncenter size-full wp-image-636" title="Lazynight | 夜阑" src="http://lazynight.me/wp-content/uploads/2011/10/20111023135638.jpg" alt="" width="517" height="252" />

<div style="background:#fdfdfd;color:black;">
</div>

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: rgb(192, 192, 192); background-color: rgb(0, 0, 0); ">
  <span style="color: rgb(105, 105, 105); ">01</span> <span style="color: rgb(255, 255, 255); "><!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" &#8220;http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd&#8221;></span> <br /><span style="color: rgb(105, 105, 105); ">02</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><html</span> <span style="color: rgb(255, 255, 0); ">xmlns=</span><span style="color: rgb(209, 56, 0); ">"http://www.w3.org/1999/xhtml"</span><span style="color: rgb(255, 68, 0); font-weight: bold; ">></span> <br /><span style="color: rgb(105, 105, 105); ">03</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><head></span><br /><span style="color: rgb(105, 105, 105); ">04</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><meta</span> <span style="color: rgb(255, 255, 0); ">http-equiv=</span><span style="color: rgb(209, 56, 0); ">"Content-Type"</span> <span style="color: rgb(255, 255, 0); ">content=</span><span style="color: rgb(209, 56, 0); ">"text/html; charset=gb2312"</span> <span style="color: rgb(255, 68, 0); font-weight: bold; ">/></span> <br /><span style="color: rgb(248, 16, 176); ">05</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><title></span>Hello Lazynight~~<span style="color: rgb(255, 68, 0); font-weight: bold; "></title></span><br /><span style="color: rgb(105, 105, 105); ">06</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><script </span><span style="color: rgb(255, 255, 0); ">type=</span><span style="color: rgb(209, 56, 0); ">"text/javascript"</span> <span style="color: rgb(255, 255, 0); ">src=</span><span style="color: rgb(209, 56, 0); ">"jquery-1.1.3.pack.js"</span><span style="color: rgb(255, 68, 0); font-weight: bold; ">></script></span> <br /><span style="color: rgb(105, 105, 105); ">07</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><script </span><span style="color: rgb(255, 255, 0); ">type=</span><span style="color: rgb(209, 56, 0); ">"text/javascript"</span><span style="color: rgb(255, 68, 0); font-weight: bold; ">></span> <br /><span style="color: rgb(105, 105, 105); ">08</span> <span style="color: rgb(192, 192, 192); ">$</span>(<span style="color: rgb(192, 192, 192); ">document</span><span style="color: rgb(192, 192, 192); ">).</span><span style="color: rgb(192, 192, 192); ">ready</span>(<span style="color: rgb(255, 68, 0); font-weight: bold; ">function</span><span style="color: rgb(192, 192, 192); ">(){</span><br /><span style="color: rgb(105, 105, 105); ">09</span> &nbsp;&nbsp;&nbsp; <span style="color: rgb(192, 192, 192); ">alert</span>(<span style="color: rgb(192, 192, 192); ">$</span>(<span style="color: rgb(209, 56, 0); ">"div"</span><span style="color: rgb(192, 192, 192); ">).</span><span style="color: rgb(192, 192, 192); ">filter</span>(<span style="color: rgb(209, 56, 0); ">"#lazynight"</span><span style="color: rgb(192, 192, 192); ">).</span><span style="color: rgb(192, 192, 192); ">html</span>());<br /><span style="color: rgb(248, 16, 176); ">10</span> &nbsp;&nbsp;&nbsp; <span style="color: rgb(192, 192, 192); ">alert</span>(<span style="color: rgb(192, 192, 192); ">$</span>(<span style="color: rgb(209, 56, 0); ">"div"</span><span style="color: rgb(192, 192, 192); ">).</span><span style="color: rgb(192, 192, 192); ">not</span>(<span style="color: rgb(209, 56, 0); ">"#lazynight"</span><span style="color: rgb(192, 192, 192); ">).</span><span style="color: rgb(192, 192, 192); ">html</span>());<br /><span style="color: rgb(105, 105, 105); ">11</span> <span style="color: rgb(192, 192, 192); ">});</span><br /><span style="color: rgb(105, 105, 105); ">12</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></script></span><br /><span style="color: rgb(105, 105, 105); ">13</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></head></span><br /><span style="color: rgb(105, 105, 105); ">14</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><body></span><br /><span style="color: rgb(248, 16, 176); ">15</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><div</span> <span style="color: rgb(255, 255, 0); ">id=</span><span style="color: rgb(209, 56, 0); ">"lazynight"</span><span style="color: rgb(255, 68, 0); font-weight: bold; ">></span>这里是 NIGHT ！<br /><span style="color: rgb(105, 105, 105); ">16</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><p></span>Hello Lazynight!<span style="color: rgb(255, 68, 0); font-weight: bold; "></p></span><br /><span style="color: rgb(105, 105, 105); ">17</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><p></span>Hello Lazynight!<span style="color: rgb(255, 68, 0); font-weight: bold; "></p></span><br /><span style="color: rgb(105, 105, 105); ">18</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></div></span><br /><span style="color: rgb(105, 105, 105); ">19</span> <br /><span style="color: rgb(248, 16, 176); ">20</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><div</span> <span style="color: rgb(255, 255, 0); ">id=</span><span style="color: rgb(209, 56, 0); ">"lazyday"</span><span style="color: rgb(255, 68, 0); font-weight: bold; ">></span>这里是 DAY ！<br /><span style="color: rgb(105, 105, 105); ">21</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><p></span>Hello Lazyday!<span style="color: rgb(255, 68, 0); font-weight: bold; "></p></span><br /><span style="color: rgb(105, 105, 105); ">22</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><p></span>Hello Lazyday!<span style="color: rgb(255, 68, 0); font-weight: bold; "></p></span><br /><span style="color: rgb(105, 105, 105); ">23</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></div></span><br /><span style="color: rgb(105, 105, 105); ">24</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "><pre></span><br /><span style="color: rgb(248, 16, 176); ">25</span> filter()能够将元素精简到只剩下满足过滤条件的那些，<br /><span style="color: rgb(105, 105, 105); ">26</span> not()恰恰相反，他移除了所有满足条件的。<br /><span style="color: rgb(105, 105, 105); ">27</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></pre></span><br /><span style="color: rgb(105, 105, 105); ">28</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></body></span><br /><span style="color: rgb(105, 105, 105); ">29</span> <span style="color: rgb(255, 68, 0); font-weight: bold; "></html></span>
</div>

<span style="color: #ff6600;"><a href="http://down.qiannao.com/space/file/flowerowl/-4e0a-4f20-5206-4eab/Lazy21_filter()-002dnot().rar/.page" target="_blank"><span style="color: #ff6600;">下载源码</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [jQuery 抄袭笔记(21) :filter()，not()][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/632.html