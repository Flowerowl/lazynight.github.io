---
title: Cocoa学习笔记（1）： 使用NSSpeechSynthesizer朗读
author: Flowerowl
layout: post
permalink: /2403.html
duoshuo_thread_id:
  - 6544881
views:
  - 1165
bot_views:
  - 3
categories:
  - Cocoa
  - Object-C
  - 代码
---
&nbsp;

新建Cocoa Application工程，然后添加一个Object-c Class，名字为：AppController。

修改AppController.h 内容，代码如下。

给window设置firstResponser：textField，让它运行即获得输入焦点。

将一个Object拖入Objects窗口：

[<img class="alignnone size-full wp-image-2404" title="Cocoa 1" src="http://lazynight.me/wp-content/uploads/2012/08/Cocoa-1.jpg" alt="" width="240" height="134" />][1]

修改为AppController类：

[<img class="alignnone size-full wp-image-2405" title="Cocoa2" src="http://lazynight.me/wp-content/uploads/2012/08/Cocoa2.jpg" alt="" width="254" height="151" />][2]

关联成员变量和动作：

[<img class="alignnone size-full wp-image-2404" title="Cocoa 1" src="http://lazynight.me/wp-content/uploads/2012/08/Cocoa-1.jpg" alt="" width="240" height="134" />][1]

修改AppController.m,代码如下。

运行：

[<img class="alignnone size-full wp-image-2407" title="Cocoa4" src="http://lazynight.me/wp-content/uploads/2012/08/Cocoa4.jpg" alt="" width="463" height="273" />][3]

&nbsp;

若想了解程序背后运行机制，推荐图书：<span style="color: #ff0000;">《Cocoa Programming for Mac OS X 》</span>

我看的是第三版，现在有第四版了貌似。

&nbsp;

<pre class="lang:default decode:true" title="AppController.h">#import &lt;Foundation/Foundation.h&gt;

@interface AppController : NSObject{
    IBOutlet NSTextField *textField;
    NSSpeechSynthesizer *speechSynth;
}

- (IBAction)sayIt:(id)sender;
- (IBAction)stopIt:(id)sender;

@end</pre>

&nbsp;

<pre class="lang:default decode:true" title="AppController.m">#import "AppController.h"

@implementation AppController

- (id)init{

    NSLog(@"init");
    speechSynth=[[NSSpeechSynthesizer alloc] initWithVoice:nil];
    return self;
}

- (IBAction)sayIt:(id)sender{
    NSString *string=[textField stringValue];
    if([string length]==0){
        NSLog(@"The textField %@ is of zero-length",textField);
        return;
    }
    [speechSynth startSpeakingString:string];
    NSLog(@"Have started to say:%@",string);
}

- (IBAction)stopIt:(id)sender{
    [speechSynth stopSpeaking];
    NSLog(@"Have stopped");
}
@end</pre>

&nbsp;

转载请注明：[于哲的博客][4] &raquo; [Cocoa学习笔记（1）： 使用NSSpeechSynthesizer朗读][5]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/Cocoa-1.jpg
 [2]: http://lazynight.me/wp-content/uploads/2012/08/Cocoa2.jpg
 [3]: http://lazynight.me/wp-content/uploads/2012/08/Cocoa4.jpg
 [4]: http://localhost/wordpress
 [5]: http://localhost/wordpress/2403.html