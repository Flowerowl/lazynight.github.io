---
title: iOS学习笔记（2）：MiniDictionary
author: Flowerowl
layout: post
permalink: /2356.html
duoshuo_thread_id:
  - 5958922
views:
  - 873
bot_views:
  - 2
categories:
  - Object-C
  - 代码
tags:
  - ios
---
功能：使用API实现翻译。

API地址：<span style="color: #ff0000;"><a href="http://api.liqwei.com/" target="_blank"><span style="color: #ff0000;">http://api.liqwei.com/</span></a></span>

[<img class="alignnone size-full wp-image-2357" title="news_20128991615" src="http://lazynight.me/wp-content/uploads/2012/08/news_20128991615.jpg" alt="" width="365" height="710" />][1]

<pre class="lang:default decode:true">- (IBAction)btnPress:(UIButton *)sender { 
    if([_textField.text length]==0){
        return;
    }
    NSStringEncoding encode=CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000);
    NSString *strURL=[[NSString stringWithFormat:@"http://api.liqwei.com/translate/?language=zh-CN|en&content=%@",_textField.text]stringByAddingPercentEscapesUsingEncoding:encode];
    NSURL *url=[NSURL URLWithString:strURL];
    NSError *err=nil;
    NSString *strResult=[NSString stringWithContentsOfURL:url encoding:encode error:&err];
    if(err){
        NSLog(@"err=%@",[err description]);
    }else {
        _lblResult.text=strResult;
    }
}</pre>

<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0g6ybicay" target="_blank"><span style="color: #ff0000;">下载源码 </span></a></span>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [iOS学习笔记（2）：MiniDictionary][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/news_20128991615.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2356.html