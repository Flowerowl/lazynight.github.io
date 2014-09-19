---
title: 利用jQuery blockUI添加遮罩层
author: Flowerowl
layout: post
permalink: /1889.html
duoshuo_thread_id:
  - 1267011
views:
  - 2180
bot_views:
  - 3
categories:
  - jQuery
  - 代码
---
<embed src="http://www.xiami.com/widget/0_387621/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent">
</embed>

今天翻看之前别人写的主题时发现的一个插件。

可以利用遮罩层弹出登录界面/表单/等待提示/图片展示等，不用自己动手写javascript了，方便很多。

看看Demo，挺好玩的。

在线Demo：<span style="color: #ff4040;"><a href="http://malsup.com/jquery/block/#demos" target="_blank"><span style="color: #ff4040;">http://malsup.com/jquery/block/#demos</span></a></span>

<img class="aligncenter size-full wp-image-1891" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/block.gif" alt="" width="453" height="232" />

<pre class="brush:js">&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
&lt;meta charset="UTF-8"/&gt;
&lt;title&gt;jquery.blockUI Demo&lt;/title&gt;
&lt;script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.7.0/jquery.min.js" &gt;&lt;/script&gt;
&lt;script type="text/javascript" src="jquery.blockUI.js"&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;script type="text/javascript"&gt;
    $(document).ready(function () {
        $("#login a").click(function () {
            $.blockUI({
                message: $("#loginForm"),
                fadeIn: 700,
                fadeOut: 700,
                showOverlay: true,
                centerY: true,
                centerX: true,
                css: {
                    width: '250px',
                    backgroundColor: '#000',
                    opacity: .6,
                    border: 'none',
                    'border-radius': '10px',
                    color: '#fff',
                    padding: '10px'
                }
            });
            $('.blockOverlay').attr('title','Click to unblock').click($.unblockUI);
        });
    });
&lt;/script&gt;
&lt;div id="contianer"&gt;
    &lt;div id="login"&gt;
        &lt;a href="#"&gt;登录&lt;/a&gt;
	&lt;/div&gt;
    &lt;div id="loginForm" style="display:none;"&gt;
		&lt;table&gt;
			&lt;tr&gt;
			    &lt;td&gt;用户名：&lt;/td&gt;
                &lt;td&gt;&lt;input id="userName" type="text" /&gt;&lt;/td&gt;
			&lt;/tr&gt;
            &lt;tr&gt;
                &lt;td&gt;密码：&lt;/td&gt;
                &lt;td&gt;&lt;input id="pwd"type="text" /&gt;&lt;/td&gt;
            &lt;/tr&gt;
            &lt;tr&gt;
                &lt;td&gt;&lt;input id="login" type="button" value="登录"/&gt;&lt;/td&gt;
                &lt;td&gt;&lt;input id="cancle" type="button" value="取消"/&gt;&lt;/td&gt;
            &lt;/tr&gt;
		&lt;/table&gt;
	&lt;/div&gt;
&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

转载请注明：[于哲的博客][1] &raquo; [利用jQuery blockUI添加遮罩层][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1889.html