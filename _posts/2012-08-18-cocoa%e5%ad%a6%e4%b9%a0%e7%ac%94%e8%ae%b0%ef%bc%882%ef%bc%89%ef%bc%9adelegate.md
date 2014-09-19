---
title: Cocoa学习笔记（2）：delegate
author: Flowerowl
layout: post
permalink: /2415.html
duoshuo_thread_id:
  - 6552163
views:
  - 1050
bot_views:
  - 2
categories:
  - Cocoa
  - Object-C
  - 代码
---
接着用上一节朗读的例子来演示。

如果你想让程序在朗读的时候使start按钮变灰，而stop按钮可以点击，那就可以通过设置speechSynth的delegate来实现效果。

speechSynth将委托（delegate）给自己，即AppController。

调用speechSynth的委托通过AppController来进行其他操作，比如设置按钮变灰。

不知道我说没说明白，我是从C#那里的委托来理解Obj-C的委托的，大致差不多吧，不对的地方帮忙提出来哦。

效果如下：

[<img title="Cocoa delegate" src="http://lazynight.me/wp-content/uploads/2012/08/Cocoa-delegate.jpg" alt="" width="480" height="166" />][1]

<pre class="lang:default decode:true">- (id)init{
    NSLog(@"init");
    speechSynth=[[NSSpeechSynthesizer alloc] initWithVoice:nil];
    [speechSynth setDelegate:self];
    return self;
}</pre>

对于NSSpeechSynthesizer的Delegate函数有：

– speechSynthesizer:willSpeakWord:ofString:  
– speechSynthesizer:willSpeakPhoneme:  
– speechSynthesizer:didEncounterErrorAtIndex:ofString:message:  
– speechSynthesizer:didEncounterSyncMessage:  
– speechSynthesizer:didFinishSpeaking:

这些可以在Document查询。

关系图：

[<img class="alignnone size-full wp-image-2421" title="delegate" src="http://lazynight.me/wp-content/uploads/2012/08/delegaet.jpg" alt="" width="515" height="468" />][2]

<pre class="lang:default decode:true" title="AppController.m">//
//  AppController.m
//  SpeakLine
//
//  Created by 哲   于 on 12-8-18.
//  Copyright (c) 2012年 哈尔滨理工大学. All rights reserved.
//

#import "AppController.h"

@implementation AppController

- (id)init{

    NSLog(@"init");
    speechSynth=[[NSSpeechSynthesizer alloc] initWithVoice:nil];
    [speechSynth setDelegate:self];
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

- (void)speechSynthesizer:(NSSpeechSynthesizer *)sender didFinishSpeaking:(BOOL)complete{
    NSLog(@"complete = %d", complete);
    [startButton setEnabled:YES];
    [stopButton setEnabled:NO];
}

- (void)speechSynthesizer:(NSSpeechSynthesizer *)sender willSpeakPhoneme:(short)phonemeOpcode{
    [startButton setEnabled:NO];
    [stopButton setEnabled:YES];
}

@end</pre>

<span style="color: #ff0000;"> <a href="http://dl.vmall.com/c09p53sf8b" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

转载请注明：[于哲的博客][3] &raquo; [Cocoa学习笔记（2）：delegate][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/Cocoa-delegate.jpg
 [2]: http://lazynight.me/wp-content/uploads/2012/08/delegaet.jpg
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2415.html