---
title: Node.js之Express框架
author: Flowerowl
layout: post
permalink: /1969.html
duoshuo_thread_id:
  - 1267021
views:
  - 13395
bot_views:
  - 2
categories:
  - Node.js
  - 代码
tags:
  - Express
---
<embed src="http://www.xiami.com/widget/0_2090583/singlePlayer.swf" type="application/x-shockwave-flash" width="257" height="33" wmode="transparent">
</embed>

OK，这篇文章主要介绍Express的部署问题，为搭建一个博客系统做好准备。

Express是Node.js上最流行的Web开发框架。

Express用起来让我想起来去年12月的时候折腾的Ruby on Rails，那时候折腾了一个周，写了一个半成的博客程序，号称“15分钟打造一个博客系统”大概就是Rails的特色了吧。不过Express就没有快速的特点了，需要慢慢来磨合。

<span style="color: #ff4040;">关系如下：</span>

<span style="color: #ff4040;">Ruby <–> Rack <–> Ruby on Rails</span>  
<span style="color: #ff4040;">node.js <–> Connect <–> express.js</span>

决定不再重蹈Ruby on Rails的覆辙，把Node.js博客系统搭建起来，如果你想和我一起学习，那么来吧。

1.Windows下安装Express模块，CMD : npm install -g express

安装完成之后可以通过express -v 查看当前版本，其他类似（比如node -v ）。

2.创建一个项目CMD: express LazyBlog,会自动生成目录。

<img class="aligncenter size-full wp-image-1972" title="express" src="http://lazynight.me/wp-content/uploads/2012/04/express.gif" alt="" width="506" height="366" />

3.CMD: CD LazyBlog （进入LazyBlog目录）

node app.js （运行程序，默认地址是http://localhost:3000）

如果打开页面出错，可能你没有安装jade模块，那就输入npm install jade进行安装，如下图

此时再次运行app.js就可以看到你最初的博客界面了。

<img class="aligncenter size-full wp-image-1971" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/Lazynight1.gif" alt="" width="847" height="549" />

&nbsp;

<img class="aligncenter size-full wp-image-1974" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/Lazynight2.gif" alt="" width="236" height="223" />

Express.js中文入门手册：<span style="color: #ff4040;"><a href="http://www.csser.com/board/4f77e6f996ca600f78000936" target="_blank"><span style="color: #ff4040;">http://www.csser.com/board/4f77e6f996ca600f78000936</span></a></span>

Express目录介绍：

<table>
  <tr>
    <td>
      目录/文件
    </td>
    
    <td>
      说明
    </td>
  </tr>
  
  <tr>
    <td>
      ./
    </td>
    
    <td>
       根目录，我们的node.js代码都会方这个目录
    </td>
  </tr>
  
  <tr>
    <td>
       package.json
    </td>
    
    <td>
        npm依赖配置文件， 类似ruby中的Gemfile, java Maven中的pom.xml文件. 一会需要在这里添加 markdown-js 项目依赖
    </td>
  </tr>
  
  <tr>
    <td>
       app.js
    </td>
    
    <td>
       项目的入口文件
    </td>
  </tr>
  
  <tr>
    <td>
       public/</p> <p>
        javascript/
      </p>
      
      <p>
        stylesheets/
      </p>
      
      <p>
        images/</td> <td>
           存放静态资源文件, jquery/prettify.js等静态库会方这里，当然自己编写的前端代码也可以放这里
        </td></tr> 
        
        <tr>
          <td>
             views/
          </td>
          
          <td>
              模板文件, express默认采用jade, 当然，你也可以使用自己喜欢的haml,JES, coffeeKup, jQueryTemplate等模板引擎
          </td>
        </tr>
        
        <tr>
          <td>
             node_modules/
          </td>
          
          <td>
             存放npm安装到本地依赖包，依赖包在package.json文件中声明，使用npm install指令安装
          </td>
        </tr></tbody> </table> 
        
        <p>
          &nbsp;
        </p>
        
        <p>
          转载请注明：<a href="http://localhost/wordpress">于哲的博客</a> &raquo; <a href="http://localhost/wordpress/1969.html">Node.js之Express框架</a>
        </p>