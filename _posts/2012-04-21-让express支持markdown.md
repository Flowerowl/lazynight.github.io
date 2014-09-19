---
title: 让Express支持Markdown
author: Flowerowl
layout: post
permalink: /1977.html
duoshuo_thread_id:
  - 1267022
views:
  - 2868
bot_views:
  - 1
categories:
  - Node.js
  - 代码
tags:
  - Express
  - Markdown
---
什么是Markdown，就是一种标记语言，能够清楚快捷的输出所要的文本内容（格式上有增强效果），支持html。Github上的readme就是用markdown来实现。今天学习一下为Node.js上Express添加Markdown功能。Express 并不直接支持markdown语法，需要为项目添加markdown-js模块的依赖。

1.修改pacage.json

原版：

<pre class="brush:js">{
    "name": "application-name"
    , "version": "0.0.1"
    , "private": true
    , "dependencies": {
      "express": "2.4.6"
    , "jade": "&gt;= 0.0.1"
  }
}</pre>

修改为：

<pre class="brush:js">{
    "name": "LazyBlog"
    , "version": "0.0.1"
    , "private": true
    , "dependencies": {
      "express": "2.4.6"
    , "jade": "&gt;= 0.0.1"
    , "markdown-js": "&gt;= 0.0.1"
  }
}</pre>

&nbsp;

然后CMD进入LazyBlog目录，安装依赖：npm install

会在LazyBlog生成node_modules 目录，里边有

<img class="aligncenter size-full wp-image-1978" title="markdown" src="http://lazynight.me/wp-content/uploads/2012/04/markdown.gif" alt="" width="156" height="118" />

&nbsp;

现在可以写代码了，打开app.js，导入markdown模块

<pre class="brush:js">var express = require('express')
  , routes = require('./routes')
  , markdown=require('markdown-js');

var app = module.exports = express.createServer();</pre>

然后给Express注册Markdown渲染器

<pre class="brush:js">app.configure('development', function(){
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
});

app.configure('production', function(){
  app.use(express.errorHandler());
});

app.register('.md',{
	compile:function(str,options){
		var html=markdown.makeHtml(str);
		return function(locals){
			return html.replace(/\{([^}]+)\}/g,function(_,name){
				return locals[name];
			});
		}
	}
});
// Routes</pre>

接着添加对markdown的路由（如果这些看起来对你很头大，那么先学学Express.js吧，上一篇文章有写过。如果不知道路由什么意思？那就先去学Node.js吧&#8230;）

<pre class="brush:js">app.get('/', routes.index);
//app.get('/md5/:string',routes.md5);
app.get('/markdown',function(req,res){
	res.render('index.md',{layout:false});
});</pre>

路由写好了，它指向index.md，用它来处理markdown语法文本，所以我们要去views目录建立一个index.md文件。

打开index.md ，现在可以写一写markdown标记的文本了，这里我把windows下markdownpad的欢迎界面内容搬过来了，你可以自己写点别的，比如

<pre class="brush:js">This is a demo page
===================
[Lazynight](http://lazynight.me \"Click\")</pre>

MarkdownPad欢迎界面

<pre class="brush:applescript"># Welcome to MarkdownPad #

**MarkdownPad** is a full-featured Markdown editor for Windows. 

## Full control over your documents ##

Want to make something **bold**? Press `Ctrl + B`.

How about *italic*? Press `Ctrl + I`.

&gt; Write a quote with `Ctrl + Q`

No matter what you're working on, you'll have quick access to Markdown syntax with handy keyboard shortcuts and toolbar buttons.

## See your changes instantly with LivePreview ##

Don't guess if your [hyperlink syntax](http://markdownpad.com) is correct; LivePreview will show you exactly what your document looks like every time you press a key.

## Make it your own ##

Fonts, sizes, color schemes, and even the HTML stylesheets are 100% customizable so you can turn MarkdownPad into your ideal editor.</pre>

&nbsp;

&nbsp;

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [让Express支持Markdown][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1977.html