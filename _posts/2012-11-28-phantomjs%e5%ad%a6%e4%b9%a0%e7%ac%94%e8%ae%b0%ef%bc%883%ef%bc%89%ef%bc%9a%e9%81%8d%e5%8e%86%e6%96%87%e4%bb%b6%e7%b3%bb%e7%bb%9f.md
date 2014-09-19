---
title: PhantomJS学习笔记（3）：遍历文件系统
author: Flowerowl
layout: post
permalink: /2723.html
views:
  - 1333
duoshuo_thread_id:
  - 1220743779864322423
bot_views:
  - 1
categories:
  - PhantomJS
  - 代码
tags:
  - PhantomJS
---
再来一个官方demo，遍历系统文件。

命令：phantomjs scandir.js /

从根目录开始递归遍历。

代码：

<pre class="lang:default decode:true">// List all the files in a Tree of Directories
var system = require('system');

if (system.args.length !== 2) {
    console.log("Usage: phantomjs scandir.js DIRECTORY_TO_SCAN");
    phantom.exit(1);
}

var scanDirectory = function (path) {
    var fs = require('fs');
    if (fs.exists(path) && fs.isFile(path)) {
        console.log(path);
    } else if (fs.isDirectory(path)) {
        fs.list(path).forEach(function (e) {
            if ( e !== "." && e !== ".." ) {    //&lt; Avoid loops
                scanDirectory(path + '/' + e);
            }
        });
    }
};
scanDirectory(system.args[1]);
phantom.exit();</pre>

<img title="dfs.gif" src="http://lazynight.me/wp-content/uploads/2012/11/dfs.gif" alt="Dfs" width="600" height="273" border="0" />

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [PhantomJS学习笔记（3）：遍历文件系统][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2723.html