---
title: PushState+Ajax 完美实现无刷新
author: Flowerowl
layout: post
permalink: /1897.html
duoshuo_thread_id:
  - 1267012
views:
  - 2761
bot_views:
  - 3
categories:
  - Ajax
  - Html5
  - 代码
tags:
  - API
  - PushState
---
折腾一下PJAX，利用HTML5的新API，实现历史记录的完美导入。

不知道你用没用过Github，里边的目录跳转就是用html5的pushstate做的，效果很酷。

还有一个关于web知识的宣传网站，<span style="color: #ff4040;"><a href="http://www.20thingsilearned.com/en-US" target="_blank"><span style="color: #ff4040;">http://www.20thingsilearned.com/en-US</span></a></span>

你可以完美的前进后退，并且可以与好友分享特定页面，实现方法？不用我说了吧。

实现PJAX只需要以下几点：

*   <span style="color: #ff4040;"><code>&lt;span style="color: #ff4040;">&lt;span>pushState(state, title, url)&lt;/span>&lt;/span> </code></span><span><span>– Add one history state into browser history and update the URL in the browser window</span></span>
*   <span style="color: #ff4040;"><code>replaceState(state, title, url)</code></span><span><span> – operates exactly like history.pushState() except that replaceState() modifies the current history entry instead of creating a new one.</span></span>
*   <span style="color: #ff4040;"><code>window.onpopstate</code></span><span><span> – A popstate event is dispatched to the window every time the active history entry changes. </span></span>  
    <span><span>If the history entry being activated was created by a call to pushState or affected by a call to replaceState, the popstate event&#8217;s state property contains a copy of the history entry&#8217;s state object.</span></span>

不想手写？拿来主义？

好吧，这里推荐给你一个现成的文件History.js，完美支持HTML4与HTML5，

在HTML5浏览器使用新API，HTML4浏览器继续锚点的干活&#8230;

<span style="color: #ff4040;"><a href="https://github.com/balupton/History.js" target="_blank"><span style="color: #ff4040;">https://github.com/balupton/History.js</span></a></span>

<span style="color: #ff4040;"><span style="color: #ff4040;">【 </span></span><span style="color: #ff4040;"><a href="http://lazynight.me/z/html5-history/index.html" target="_blank"><span style="color: #ff4040;">在线演示</span></a> <span style="color: #ff4040;">】</span></span>

试了一下，把wp主题给整了一个PJAX版本，效果不错，继续挖掘中。

想折腾的朋友，可以开始动手了。

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [PushState+Ajax 完美实现无刷新][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1897.html