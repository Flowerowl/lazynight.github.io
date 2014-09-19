---
title: Bootstrap 小试：不喜欢折腾界面的程序员的福音
author: Flowerowl
layout: post
permalink: /2728.html
views:
  - 4083
duoshuo_thread_id:
  - 1220743779864322426
bot_views:
  - 1
categories:
  - 技术杂谈
tags:
  - Bootstrap.js
  - CSS
  - Html5
  - Javascript
  - 前端
---
折腾Backbone.js的时候偶然发现了Bootstrap，刺激我像个爬虫，再一次伸向了未知的区域······

时隔5个月，我又找到了折腾新鲜事物的感觉，不过时间已经不多，寒假回去准备考研，就没时间这么折腾了。

还有好多东西没有尝试，可惜时间不够了。

ok，废话不多说，唠叨到此为止，下边来看看Bootstrap：

学习资料：

[http://twitter.github.com/bootstrap/getting-started.html][1]

<http://wrongwaycn.github.com/bootstrap/docs/index.html>

Github:

<https://github.com/twitter/bootstrap/>

> Bootstrap是快速开发Web应用程序的前端工具包。它是一个CSS和HTML的集合，它使用了最新的浏览器技术，给你的Web开发提供了时尚的版式，表单，buttons，表格，网格系统等等。

2011年，twitter的“一小撮”工程师为了提高他们内部的分析和管理能力，用业余时间为他们的产品构建了一套易用、优雅、灵活、可扩展的前端工具集&#8211;[BootStrap][2]。Bootstrap由[MARK OTTO][3]和<a href="http://twitter.com/fat" target="_blank">Jacob Thornton</a>所设计和建立，在[github][4]上开源之后，迅速成为该站上最多人[watch][5]&[fork][6]的项目。大量工程师踊跃为该项目贡献代码，社区惊人地活跃，代码版本进化非常快速，官方文档质量极其高(可以说是优雅)，同时涌现了许多基于Bootstrap建设的网站：界面清新、简洁;要素排版利落大方。

总的来说就是按照规定的div标签，css还有js的部分可以不写或者少些很多，提高效率~

下边是我参考官方demo自己写的一个html，效果如下

[<img class="alignnone size-large wp-image-2730" title="bootstrap" src="http://lazynight.me/wp-content/uploads/2012/11/bootstrap-1024x574.gif" alt="" width="1024" height="574" />][7]

是不是显得立马上档次了？其实你只需要按照相应标签写写html就好。

另外，我相信你已经发现了上图里的滚动大图，自己根本不需要写js~！

代码：

<pre class="lang:default decode:true">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
  &lt;head&gt;
    &lt;meta charset="utf-8"&gt;
    &lt;title&gt;Bootstrap, from Twitter&lt;/title&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
    &lt;meta name="description" content=""&gt;
    &lt;meta name="author" content=""&gt;

    &lt;link href="../assets/css/bootstrap.css" rel="stylesheet"&gt;
    &lt;link href="../assets/css/bootstrap-responsive.css" rel="stylesheet"&gt;

    &lt;!-- HTML5 shim, for IE6-8 support of HTML5 elements --&gt;
    &lt;!--[if lt IE 9]&gt;
      &lt;script src="http://html5shim.googlecode.com/svn/trunk/html5.js"&gt;&lt;/script&gt;
    &lt;![endif]--&gt;

  &lt;/head&gt;

  &lt;body&gt;

    &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt;
      &lt;div class="navbar-inner"&gt;
        &lt;div class="container"&gt;
          &lt;a class="btn btn-navbar" data-toggle="collapse" data-target=".nav-collapse"&gt;
            &lt;span class="icon-bar"&gt;&lt;/span&gt;
            &lt;span class="icon-bar"&gt;&lt;/span&gt;
            &lt;span class="icon-bar"&gt;&lt;/span&gt;
          &lt;/a&gt;
          &lt;a class="brand" href="#"&gt;Lazynight&lt;/a&gt;
          &lt;div class="nav-collapse collapse"&gt;
            &lt;ul class="nav"&gt;
              &lt;li class="active"&gt;&lt;a href="#"&gt;Home&lt;/a&gt;&lt;/li&gt;
              &lt;li class="dropdown"&gt;
                &lt;a href="#" class="dropdown-toggle" data-toggle="dropdown"&gt;Dropdown &lt;b class="caret"&gt;&lt;/b&gt;&lt;/a&gt;
                &lt;ul class="dropdown-menu"&gt;
                  &lt;li&gt;&lt;a href="#"&gt;Music&lt;/a&gt;&lt;/li&gt;
                  &lt;li&gt;&lt;a href="#"&gt;Movie&lt;/a&gt;&lt;/li&gt;
                  &lt;li&gt;&lt;a href="#"&gt;Book&lt;/a&gt;&lt;/li&gt;
                  &lt;li class="divider"&gt;&lt;/li&gt;
                  &lt;li class="nav-header"&gt;Style&lt;/li&gt;
                  &lt;li&gt;&lt;a href="#"&gt;Indie&lt;/a&gt;&lt;/li&gt;
                  &lt;li&gt;&lt;a href="#"&gt;Rock&lt;/a&gt;&lt;/li&gt;
                &lt;/ul&gt;
              &lt;/li&gt;
            &lt;/ul&gt;
            &lt;form class="navbar-form pull-right"&gt;
              &lt;input class="span2" type="text" placeholder="Email"&gt;
              &lt;input class="span2" type="password" placeholder="Password"&gt;
              &lt;button type="submit" class="btn"&gt;Sign in&lt;/button&gt;
            &lt;/form&gt;
          &lt;/div&gt;
        &lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;

    &lt;div id="myCarousel" class="carousel slide"&gt;
      &lt;div class="carousel-inner"&gt;
        &lt;div class="item active"&gt;
          &lt;img src="../assets/img/examples/slide-01.jpg" alt=""&gt;
        &lt;/div&gt;
        &lt;div class="item"&gt;
          &lt;img src="../assets/img/examples/slide-02.jpg" alt=""&gt;
        &lt;/div&gt;
        &lt;div class="item"&gt;
          &lt;img src="../assets/img/examples/slide-03.jpg" alt=""&gt;
        &lt;/div&gt;
      &lt;/div&gt;
      &lt;a class="left carousel-control" href="#myCarousel" data-slide="prev"&gt;&lsaquo;&lt;/a&gt;
      &lt;a class="right carousel-control" href="#myCarousel" data-slide="next"&gt;&rsaquo;&lt;/a&gt;
    &lt;/div&gt;

    &lt;div class="container"&gt;
      &lt;div class="row"&gt;
        &lt;div class="span4"&gt;
          &lt;h2&gt;Heading&lt;/h2&gt;
          &lt;p&gt;Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus. Etiam porta sem malesuada magna mollis euismod. Donec sed odio dui. &lt;/p&gt;
          &lt;p&gt;&lt;a class="btn" href="#"&gt;View details &raquo;&lt;/a&gt;&lt;/p&gt;
        &lt;/div&gt;
        &lt;div class="span4"&gt;
          &lt;h2&gt;Heading&lt;/h2&gt;
          &lt;p&gt;Donec id elit non mi porta gravida at eget metus. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus. Etiam porta sem malesuada magna mollis euismod. Donec sed odio dui. &lt;/p&gt;
          &lt;p&gt;&lt;a class="btn" href="#"&gt;View details &raquo;&lt;/a&gt;&lt;/p&gt;
       &lt;/div&gt;
        &lt;div class="span4"&gt;
          &lt;h2&gt;Heading&lt;/h2&gt;
          &lt;p&gt;Donec sed odio dui. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Vestibulum id ligula porta felis euismod semper. Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.&lt;/p&gt;
          &lt;p&gt;&lt;a class="btn" href="#"&gt;View details &raquo;&lt;/a&gt;&lt;/p&gt;
        &lt;/div&gt;
      &lt;/div&gt;

      &lt;hr&gt;

      &lt;footer&gt;
        &lt;p&gt;&copy; Lazynight 2012&lt;/p&gt;
      &lt;/footer&gt;

    &lt;/div&gt; 

    &lt;script src="../assets/js/jquery.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-transition.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-alert.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-modal.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-dropdown.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-scrollspy.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-tab.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-tooltip.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-popover.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-button.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-collapse.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-carousel.js"&gt;&lt;/script&gt;
    &lt;script src="../assets/js/bootstrap-typeahead.js"&gt;&lt;/script&gt;

  &lt;/body&gt;
&lt;/html&gt;</pre>

&nbsp;

参考文献与延伸阅读：

1.Bootstrap的来由和发展。<http://www.alistapart.com/articles/building-twitter-bootstrap/>

2.Less与Sass的介绍与对比.<http://coding.smashingmagazine.com/2011/09/09/an-introduction-to-less-and-comparison-to-sass/>

3.Html5模板 <http://html5boilerplate.com/>

4.Html5与Bootstrap混合项目(H5BP)<https://gist.github.com/1422879>

5.20个有用的Bootstrap资源 <http://www.webresourcesdepot.com/20-beautiful-resources-that-complement-twitter-bootstrap/>

6.Bootstrap按钮生成器 <http://charliepark.org/bootstrap_buttons/>

7.前后端结合讨论  <http://stackoverflow.com/questions/9525170/backend-technology-for-front-end-technologies-like-twitter-bootstrap>

8. Bootstrap英文教程  <http://webdesign.tutsplus.com/tutorials/htmlcss-tutorials/stepping-out-with-bootstrap-from-twitter/>

转载请注明：[于哲的博客][8] &raquo; [Bootstrap 小试：不喜欢折腾界面的程序员的福音][9]

 [1]: http://wrongwaycn.github.com/bootstrap/docs/index.html
 [2]: http://twitter.github.com/bootstrap/
 [3]: http://twitter.com/mdo
 [4]: https://github.com/
 [5]: https://github.com/twitter/bootstrap/watchers
 [6]: https://github.com/twitter/bootstrap/network/members
 [7]: http://lazynight.me/wp-content/uploads/2012/11/bootstrap.gif
 [8]: http://localhost/wordpress
 [9]: http://localhost/wordpress/2728.html