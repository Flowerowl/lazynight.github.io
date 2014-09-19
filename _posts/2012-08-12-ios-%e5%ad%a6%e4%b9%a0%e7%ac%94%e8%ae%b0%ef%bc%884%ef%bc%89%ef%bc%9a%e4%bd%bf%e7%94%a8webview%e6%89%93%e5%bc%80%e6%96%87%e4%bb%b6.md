---
title: iOS 学习笔记（4）：使用webView打开文件
author: Flowerowl
layout: post
permalink: /2365.html
duoshuo_thread_id:
  - 6171346
views:
  - 998
bot_views:
  - 3
categories:
  - Object-C
  - 代码
tags:
  - ios
---
使用webview打开doc、pdf、xls、网址

<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0enylo5lt" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

[<img class="alignnone size-full wp-image-2366" title="webview" src="http://lazynight.me/wp-content/uploads/2012/08/webview.jpg" alt="" width="319" height="286" />][1]

[<img class="alignnone size-full wp-image-2367" title="webview2" src="http://lazynight.me/wp-content/uploads/2012/08/webview2.jpg" alt="" width="320" height="478" />][2]

&nbsp;

<pre class="lang:default decode:true ">//
//  ViewController.m
//  MyWeb
//
//  Created by 哲   于 on 12-8-12.
//  Copyright (c) 2012年 哈尔滨理工大学. All rights reserved.
//

#import "ViewController.h"
#import "XWebViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
	// Do any additional setup after loading the view, typically from a nib.
}

- (void)viewDidUnload
{
    [super viewDidUnload];
    // Release any retained subviews of the main view.
}

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    if ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone) {
        return (interfaceOrientation != UIInterfaceOrientationPortraitUpsideDown);
    } else {
        return YES;
    }
}

- (IBAction)openWord:(id)sender {
    XWebViewController *web=[[[XWebViewController alloc]init]autorelease];
    web.theURL=[NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"doc" ofType:@"doc"]];
    [self presentModalViewController:web animated:YES];
}

- (IBAction)openPDF:(id)sender {
    XWebViewController *web=[[[XWebViewController alloc]init]autorelease];
    web.theURL=[NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"pdf" ofType:@"pdf"]];
    [self presentModalViewController:web animated:YES];
}

- (IBAction)openExcel:(id)sender {
    XWebViewController *web=[[[XWebViewController alloc]init]autorelease];
    web.theURL=[NSURL fileURLWithPath:[[NSBundle mainBundle] pathForResource:@"xls" ofType:@"xls"]];
    [self presentModalViewController:web animated:YES];
}

- (IBAction)openURL:(id)sender {
    XWebViewController *web=[[[XWebViewController alloc]init]autorelease];
    web.theURL=[NSURL URLWithString:@"http://lazynight.me"];
    [self presentModalViewController:web animated:YES];
}
@end</pre>

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [iOS 学习笔记（4）：使用webView打开文件][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/webview.jpg
 [2]: http://lazynight.me/wp-content/uploads/2012/08/webview2.jpg
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2365.html