---
title: iOS学习笔记（6）:滚动视图
author: Flowerowl
layout: post
permalink: /2373.html
duoshuo_thread_id:
  - 6192237
views:
  - 935
bot_views:
  - 6
categories:
  - Object-C
  - 代码
tags:
  - ios
---
<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0qmj5kpco" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

[<img class="alignnone size-full wp-image-2374" title="scrollView" src="http://lazynight.me/wp-content/uploads/2012/08/scrollView.jpg" alt="" width="320" height="477" />][1]

<pre class="lang:default decode:true ">- (void)viewDidLoad
{
    [super viewDidLoad];
    _scrollView.contentSize=CGSizeMake(320*5, 425.);
    for(int i=1;i&lt;=5;i++){
        UIImageView *imgView=[[[UIImageView alloc]initWithImage:[UIImage imageNamed:[NSString stringWithFormat:@"%d.jpg",i]]] autorelease];
        imgView.frame=CGRectMake((i-1)*320, 0.0, 320.0, 425.0);
        [_scrollView addSubview:imgView];

    }
	// Do any additional setup after loading the view, typically from a nib.
}</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [iOS学习笔记（6）:滚动视图][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/scrollView.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2373.html