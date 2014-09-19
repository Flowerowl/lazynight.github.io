---
title: iOS 学习笔记（3）：MiniDictionary完整版
author: Flowerowl
layout: post
permalink: /2360.html
duoshuo_thread_id:
  - 6136927
views:
  - 804
bot_views:
  - 3
categories:
  - Object-C
  - 代码
tags:
  - ios
---
添加：

1.手势功能，向下滑动

2.导航栏

3.英中&中英

4.登陆动画，程序图标，完善。

<span style="color: #ff0000;"><a href="http://dl.dbank.com/c035dmmy1j" target="_blank"><span style="color: #ff0000;"> 下载源码</span></a></span>

&nbsp;

[<img class="alignnone size-full wp-image-2361" title="MiniDict" src="http://lazynight.me/wp-content/uploads/2012/08/MiniDict.jpg" alt="" width="368" height="716" />][1]

<pre class="lang:default decode:true">#import "ViewController.h"
#import "XAbout.h"

@interface ViewController ()

@end

@implementation ViewController

@synthesize _btnExpress;
@synthesize _textField;
@synthesize _lblResult;

bool _chineseToEnglish;

- (void)viewDidLoad
{
    [super viewDidLoad];
	// Do any additional setup after loading the view, typically from a nib.
}

- (void)viewDidUnload
{
    [self setBtnTranslate:nil];
    [self set_textField:nil];
    [self set_lblResult:nil];
    [self set_btnExpress:nil];
    [super viewDidUnload];
    NSUserDefaults *ud=[NSUserDefaults standardUserDefaults];
    _chineseToEnglish=[ud boolForKey:@"cn2en"];
    if(_chineseToEnglish){
        _btnExpress.title=@"中英";
    }
    NSString *find=[ud stringForKey:@"findValue"];
    if(find){
        _textField.text=find;
    }
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

- (void)dealloc {
    [_textField release];
    [_lblResult release];
    [_lblResult release];
    [_btnExpress release];
    [super dealloc];
}

- (IBAction)hideKeyBorad:(id)sender {
    [_textField resignFirstResponder];
}

- (IBAction)btnPress:(UIButton *)sender { 
    if([_textField isFirstResponder]){
        [_textField resignFirstResponder];
    }
    if([_textField.text length]==0){
        return;
    }
    [[NSUserDefaults standardUserDefaults] setObject:_textField.text forKey:@"findValue"];
    NSStringEncoding encode=CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000);
    NSString *strURL=nil;
    if(_chineseToEnglish){
        strURL=[[NSString stringWithFormat:@"http://api.liqwei.com/translate/?language=zh-CN|en&content=%@",_textField.text]stringByAddingPercentEscapesUsingEncoding:encode];
    }else {
        strURL=[[NSString stringWithFormat:@"http://api.liqwei.com/translate/?language=en|zh-CN&content=%@",_textField.text]stringByAddingPercentEscapesUsingEncoding:encode];
    }
    NSUserDefaults *ud=[NSUserDefaults standardUserDefaults];
    [ud setBool:_chineseToEnglish forKey:@"cn2en"];

    NSURL *url=[NSURL URLWithString:strURL];
    NSError *err=nil;
    NSString *strResult=[NSString stringWithContentsOfURL:url encoding:encode error:&err];
    if(err){
        NSLog(@"err=%@",[err description]);
    }else {
        _lblResult.text=strResult;
        NSString *path=[NSHomeDirectory() stringByAppendingPathComponent:@"userHistory.html"];
        [strResult writeToFile:path atomically:YES encoding:encode
                         error:nil];
    }
}

- (IBAction)_btnCE:(UIBarButtonItem *)sender {
    if(_chineseToEnglish){
        _chineseToEnglish=NO;
        sender.title=@"英中";
    }else {
        _chineseToEnglish=YES;
        sender.title=@"中英";
    }

}

- (IBAction)btnAbout:(id)sender {
    XAbout *about=[[XAbout alloc]initWithNibName:@"XAbout" bundle:nil];
    //about.modalPresentationStyle=UIModalTransitionStylePartialCurl;
    [self presentModalViewController:about animated:YES];
}

-(BOOL) textFieldShouldReturn:(UITextField *)textField{

    return YES;
}

@end</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [iOS 学习笔记（3）：MiniDictionary完整版][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/MiniDict.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2360.html