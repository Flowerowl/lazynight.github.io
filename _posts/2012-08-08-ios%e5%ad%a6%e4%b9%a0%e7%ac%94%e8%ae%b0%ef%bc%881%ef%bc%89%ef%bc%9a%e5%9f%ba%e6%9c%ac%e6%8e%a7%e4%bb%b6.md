---
title: iOS学习笔记（1）：基本控件
author: Flowerowl
layout: post
permalink: /2352.html
duoshuo_thread_id:
  - 5893374
views:
  - 917
bot_views:
  - 2
categories:
  - 代码
tags:
  - ios
---
好久没写博客了，假期就鼓捣Macbook了，学学Object-C，这玩意这么火，让人有征服的冲动。

内容：熟悉一下基本控件，实现方法。

参照教程：<span style="color: #ff0000;"><a href="http://www.tudou.com/listplay/ICHpDsjAHMk/m4nkwDQwLAg.html" target="_blank"><span style="color: #ff0000;">http://www.tudou.com/listplay/ICHpDsjAHMk/m4nkwDQwLAg.html</span></a></span>

[<img class="alignnone size-full wp-image-2353" title="ios2" src="http://lazynight.me/wp-content/uploads/2012/08/ios2.jpg" alt="" width="368" height="716" />][1]

ViewController.h

<pre class="lang:default decode:true ">#import &lt;UIKit/UIKit.h&gt;

@interface ViewController : UIViewController
@property (weak, nonatomic) IBOutlet UILabel *_lblinfo;
- (IBAction)pressButton:(id)sender;
- (IBAction)playOrStop:(UISwitch *)sender;
- (IBAction)pressState:(UISegmentedControl *)sender;
@property (weak, nonatomic) IBOutlet UIImageView *_imageView;
@end</pre>

ViewController.m

<pre class="lang:default decode:true ">#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController
@synthesize _imageView;
@synthesize _lblinfo;

- (void)viewDidLoad
{
    [super viewDidLoad];
	// Do any additional setup after loading the view, typically from a nib.
    _lblinfo.text=@"Nothing";
    _lblinfo.textColor=[UIColor redColor];
    UIImage *img1=[UIImage imageNamed:@"dog.jpg"];
    UIImage *img2=[UIImage imageNamed:@"sheji.gif"];
    _imageView.animationImages=[NSArray arrayWithObjects:img1,img2,nil];
    _imageView.animationDuration=0.5;
    [_imageView startAnimating];
}

- (void)viewDidUnload
{
    [self set_lblinfo:nil];
    [self set_imageView:nil];
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
//Round Rect Button
- (IBAction)pressButton:(id)sender {
//    _lblinfo.text=@"你按下了左键";
   NSString *title=[sender titleForState:UIControlStateNormal];
//    if([title isEqualToString:@"左"])
//       {
//           _lblinfo.text=@"你按下了左键";
//       }
//    else
//       {
//           _lblinfo.text=@"你按下了右键";
//       }
//    NSString *s=[[NSString alloc]initWithFormat:@"你按下了%@键",title];
//    _lblinfo.text=s;
      NSString *s=[NSString stringWithFormat:@"你按下了%@键",title];
    _lblinfo.text=s;

}
//Switch
- (IBAction)playOrStop:(UISwitch *)sender {
    if(sender.on){
        [_imageView startAnimating];
    }else{
        [_imageView stopAnimating];
    }
}
//Segmented Control
- (IBAction)pressState:(UISegmentedControl *)sender {
    switch (sender.selectedSegmentIndex) {
        case 0:
            _imageView.animationDuration=0.1f; 
            break;
        case 2:
            _imageView.animationDuration=1.0f;
            break;
        default:
            _imageView.animationDuration=0.5f;
            break;
    }
    [_imageView startAnimating];
}

@end</pre>

<span style="color: #ff0000;"><a href="http://dl.dbank.com/c06n5mi26b" target="_blank"><span style="color: #ff0000;"> 下载源码</span></a></span>

转载请注明：[于哲的博客][2] &raquo; [iOS学习笔记（1）：基本控件][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/ios2.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2352.html