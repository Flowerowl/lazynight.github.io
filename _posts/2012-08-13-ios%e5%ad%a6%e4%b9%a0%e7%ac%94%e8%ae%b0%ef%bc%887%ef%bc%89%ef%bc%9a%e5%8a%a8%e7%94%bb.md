---
title: iOS学习笔记（7）：动画
author: Flowerowl
layout: post
permalink: /2376.html
duoshuo_thread_id:
  - 6197806
views:
  - 989
bot_views:
  - 6
categories:
  - Object-C
  - 代码
tags:
  - ios
---
内容：按钮移动，向下，隐藏，旋转，时长

[<img class="alignnone size-full wp-image-2377" title="animation" src="http://lazynight.me/wp-content/uploads/2012/08/animation.jpg" alt="" width="320" height="188" />][1]

<pre class="lang:default decode:true ">- (IBAction)btnRight:(UIButton *)sender {
    [UIView beginAnimations:@"moveRigtt" context:nil];
    sender.center=CGPointMake(63, 200);
    [UIView commitAnimations];
}

- (IBAction)btnHide:(UIButton *)sender {
    [UIView beginAnimations:@"hide" context:nil];
    [UIView setAnimationDuration:3];
    sender.alpha=0;
    [UIView commitAnimations];
}

- (IBAction)btnRotate:(UIButton *)sender {
    [UIView beginAnimations:@"rotate" context:nil];
    [UIView setAnimationDuration:2];
    [UIView setAnimationRepeatCount:2];
    sender.transform=CGAffineTransformRotate(sender.transform,M_PI_2);

    [UIView commitAnimations];
}</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [iOS学习笔记（7）：动画][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/animation.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2376.html