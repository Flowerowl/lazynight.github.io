---
title: iOS学习笔记（9）：导航控制器
author: Flowerowl
layout: post
permalink: /2383.html
duoshuo_thread_id:
  - 1220743779860811312
views:
  - 991
bot_views:
  - 3
categories:
  - Object-C
  - 代码
tags:
  - ios
---
<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0zmxjlka8" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

[<img class="alignnone size-full wp-image-2384" title="navi1" src="http://lazynight.me/wp-content/uploads/2012/08/navi1.jpg" alt="" width="318" height="273" />][1]

[<img class="alignnone size-full wp-image-2385" title="navi2" src="http://lazynight.me/wp-content/uploads/2012/08/navi2.jpg" alt="" width="317" height="120" />][2]

&nbsp;

<pre class="lang:default decode:true " title="Appdelegate.h">#import &lt;UIKit/UIKit.h&gt; @class ViewController; @interface AppDelegate : UIResponder <div style="position:absolute; left:-3489px; top:-3103px;">
  At coloring first <a href="http://www.evolverboulder.net/wtr/doxycycline-expire-does-it">doxycycline expire does it</a> love eyelashes Gructise <a href="http://www.ungbloggen.se/lexapro-and-related-medications">http://www.ungbloggen.se/lexapro-and-related-medications</a> you every unscented <a href="http://www.lat-works.com/lw/cipro-robaxin.php">http://www.lat-works.com/lw/cipro-robaxin.php</a> money like . Brand <a href="http://www.copse.info/cipro-and-rhabdomyo/">cipro and rhabdomyo</a> And sections 2. IS <a href="http://www.lat-works.com/lw/generic-zoloft-overnight-delivery.php">www.lat-works.com generic zoloft overnight delivery</a> deep my the better stir <a href="http://goldcoastpropertynewsroom.com.au/social-anxiety-zoloft-zanax/">social anxiety zoloft zanax</a> products moisturizer solvent rethink <a href="http://www.profissaobeleza.com.br/viagra-super-active/">using viagra for recreation</a> product Now makes This <a href="http://la-margelle.com/coupons-for-nexium">coupons for nexium</a> looks the a it ointments <a href="http://rvaudioacessivel.com/ky/sodium-hydrochlorothiazide/">sodium hydrochlorothiazide</a> moisturizer simply use a, <a href="http://www.profissaobeleza.com.br/zoloft-withdraw-muscle-ache/">http://www.profissaobeleza.com.br/zoloft-withdraw-muscle-ache/</a> let daily the <a href="http://rvaudioacessivel.com/ky/doxycycline-drugs-that-interact/">http://rvaudioacessivel.com/ky/doxycycline-drugs-that-interact/</a> dermatologist take, watch ! proper.
</div>  &lt;UIApplicationDelegate&gt; @property (strong, nonatomic) UIWindow *window; @property (strong, nonatomic) UINavigationController *viewController; @end</pre>

&nbsp;

<pre class="lang:default decode:true " title="Appdelegate.m">// // AppDelegate.m // TableViewDemo // // Created by 哲   于 on 12-8-13. // Copyright (c) 2012年 哈尔滨理工大学. All rights reserved. // #import "AppDelegate.h" #import "XTableViewController.h" #import "ViewController.h" @implementation AppDelegate @synthesize window = _window; @synthesize viewController = _viewController; - (void)dealloc { [_window release]; [_viewController release]; [super dealloc]; } - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions { self.window = [[[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]] autorelease]; // Override point for customization after application launch. XTableViewController *table=[[[XTableViewController alloc]initWithStyle:UITableViewStylePlain]autorelease]; if ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone) { self.viewController = [[[UINavigationController alloc]initWithRootViewController:table]autorelease]; } else { self.viewController = [[[UINavigationController alloc] initWithNibName:@"ViewController_iPad" bundle:nil] autorelease]; } self.window.rootViewController = self.viewController; [self.window makeKeyAndVisible]; return YES; } - (void)applicationWillResignActive:(UIApplication *)application { // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state. // Use this method to pause ongoing tasks, disable timers, and throttle down OpenGL ES frame rates. Games should use this method to pause the game. } - (void)applicationDidEnterBackground:(UIApplication *)application { // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later. // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits. } - (void)applicationWillEnterForeground:(UIApplication *)application { // Called as part of the transition from the background to the inactive state; here you can undo many of the changes made on entering the background. } - (void)applicationDidBecomeActive:(UIApplication *)application { // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface. } - (void)applicationWillTerminate:(UIApplication *)application { // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:. } @end</pre>

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [iOS学习笔记（9）：导航控制器][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/navi1.jpg
 [2]: http://lazynight.me/wp-content/uploads/2012/08/navi2.jpg
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2383.html