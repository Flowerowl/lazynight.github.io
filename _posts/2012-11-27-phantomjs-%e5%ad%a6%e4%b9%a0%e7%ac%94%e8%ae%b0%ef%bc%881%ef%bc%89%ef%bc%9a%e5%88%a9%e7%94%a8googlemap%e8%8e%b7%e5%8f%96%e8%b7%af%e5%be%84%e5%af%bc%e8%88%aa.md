---
title: PhantomJS 学习笔记（1）：利用googlemap获取路径导航
author: Flowerowl
layout: post
permalink: /2712.html
views:
  - 1398
duoshuo_thread_id:
  - 1220743779864322418
bot_views:
  - 1
categories:
  - PhantomJS
  - 代码
tags:
  - Google Map
  - PhantomJS
  - 路径导航
---
闲来无事，网上东瞅西瞅，偶尔碰到PhantomJS，一股新鲜感油然而生，遂折腾，此文为记。

官网：http://phantomjs.org/

文档：https://github.com/ariya/phantomjs/wiki

Github : https://github.com/ariya/phantomjs

关于PhantomJS：

> PhantomJS (<a href="http://phantomjs.org" target="_blank">www.phantomjs.org</a>) is a headless WebKit scriptable with JavaScript or CoffeeScript. It is used by hundreds of developers and dozens of organizations for web-related development workflow.
> 
> The latest stable release is version 1.7 (codenamed &#8220;Blazing Star&#8221;). Follow the official Twitter stream @PhantomJS to get the frequent development updates.
> 
> **Note**: Please **do not** create a GitHub pull request **without** reading the Contribution Guide first. Failure to do so may result in the rejection of the pull request.

意思就是：phantomjs* *是 一个基于js的webkit内核无头浏览器 也就是没有显示界面的浏览器，这样访问网页就省去了浏览器的界面绘制所消耗的系统资源，比较适合用于网络测试等应用 。

下边贴出官方的一个demo：

&nbsp;

<pre class="lang:default decode:true">// Get driving direction using Google Directions API.

var page = require('webpage').create(),
    system = require('system'),
    origin, dest, steps;

if (system.args.length &lt; 3) {
    console.log('Usage: direction.js origin destination');
    console.log('Example: direction.js "San Diego" "Palo Alto"');
    phantom.exit(1);
} else {
    origin = system.args[1];
    dest = system.args[2];
    page.open(encodeURI('http://maps.googleapis.com/maps/api/directions/xml?origin=' + origin +
                '&destination=' + dest + '&units=imperial&mode=driving&sensor=false'), function (status) {
        if (status !== 'success') {
            console.log('Unable to access network');
        } else {
            steps = page.content.match(/&lt;html_instructions&gt;(.*)&lt;\/html_instructions&gt;/ig);
            if (steps == null) {
                console.log('No data available for ' + origin + ' to ' + dest);
            } else {
                steps.forEach(function (ins) {
                    ins = ins.replace(/\&lt;/ig, '&lt;').replace(/\&gt;/ig, '&gt;');
                    ins = ins.replace(/\&lt;div/ig, '\n&lt;div');
                    ins = ins.replace(/&lt;.*?&gt;/g, '');
                    console.log(ins);
                });
                console.log('');
                console.log(page.content.match(/&lt;copyrights&gt;.*&lt;\/copyrights&gt;/ig).join('').replace(/&lt;.*?&gt;/g, ''));
            }
        }
        phantom.exit();
    });
}</pre>

运行截图：

[<img class="alignnone size-full wp-image-2713" title="phantomjs" src="http://lazynight.me/wp-content/uploads/2012/11/phantomjs.jpg" alt="" width="550" height="774" />][1]

更多好玩的demo自己可以下载源码来折腾~

玩完以后可以自己写个app，搞个什么好捏？

转载请注明：[于哲的博客][2] &raquo; [PhantomJS 学习笔记（1）：利用googlemap获取路径导航][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/11/phantomjs.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2712.html