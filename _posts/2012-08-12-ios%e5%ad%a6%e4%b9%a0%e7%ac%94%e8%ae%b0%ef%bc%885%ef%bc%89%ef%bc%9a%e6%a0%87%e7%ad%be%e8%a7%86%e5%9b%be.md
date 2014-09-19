---
title: iOS学习笔记（5）：标签视图
author: Flowerowl
layout: post
permalink: /2369.html
duoshuo_thread_id:
  - 6177343
views:
  - 770
bot_views:
  - 3
categories:
  - Object-C
  - 代码
tags:
  - ios
---
[<img class="alignnone size-full wp-image-2370" title="new project" src="http://lazynight.me/wp-content/uploads/2012/08/new-project.jpg" alt="" width="728" height="491" />][1]

[<img class="alignnone size-full wp-image-2371" title="viewtap" src="http://lazynight.me/wp-content/uploads/2012/08/viewtap.jpg" alt="" width="317" height="480" />][2]

<pre class="lang:default decode:true ">//
//  AppDelegate.m
//  TabBar
//
//  Created by 哲   于 on 12-8-12.
//  Copyright (c) 2012年 哈尔滨理工大学. All rights reserved.
//

#import "AppDelegate.h"

#import "FirstViewController.h"

#import "SecondViewController.h"

#import "ThridViewController.h"

@implementation AppDelegate

@synthesize window = _window;
@synthesize tabBarController = _tabBarController;

- (void)dealloc
{
    [_window release];
    [_tabBarController release];
    [super dealloc];
}

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]] autorelease];
    // Override point for customization after application launch.
    UIViewController *viewController1, *viewController2 ,*viewController3;

    if ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone) {
        viewController1 = [[[FirstViewController alloc] initWithNibName:@"FirstViewController_iPhone" bundle:nil] autorelease];
        viewController2 = [[[SecondViewController alloc] initWithNibName:@"SecondViewController_iPhone" bundle:nil] autorelease];
        viewController3 = [[[SecondViewController alloc] initWithNibName:@"ThridViewController" bundle:nil] autorelease];

    } else {
        viewController1 = [[[FirstViewController alloc] initWithNibName:@"FirstViewController_iPad" bundle:nil] autorelease];
        viewController2 = [[[SecondViewController alloc] initWithNibName:@"SecondViewController_iPad" bundle:nil] autorelease];

    }
    self.tabBarController = [[[UITabBarController alloc] init] autorelease];
    self.tabBarController.viewControllers = [NSArray arrayWithObjects:viewController1, viewController2,  viewController3,nil];
    self.window.rootViewController = self.tabBarController;
    [self.window makeKeyAndVisible];
    return YES;
}

- (void)applicationWillResignActive:(UIApplication *)application
{
    // Sent when the application is about to move from active to inactive state. This can occur for certain types of temporary interruptions (such as an incoming phone call or SMS message) or when the user quits the application and it begins the transition to the background state.
    // Use this method to pause ongoing tasks, disable timers, and throttle down OpenGL ES frame rates. Games should use this method to pause the game.
}

- (void)applicationDidEnterBackground:(UIApplication *)application
{
    // Use this method to release shared resources, save user data, invalidate timers, and store enough application state information to restore your application to its current state in case it is terminated later. 
    // If your application supports background execution, this method is called instead of applicationWillTerminate: when the user quits.
}

- (void)applicationWillEnterForeground:(UIApplication *)application
{
    // Called as part of the transition from the background to the inactive state; here you can undo many of the changes made on entering the background.
}

- (void)applicationDidBecomeActive:(UIApplication *)application
{
    // Restart any tasks that were paused (or not yet started) while the application was inactive. If the application was previously in the background, optionally refresh the user interface.
}

- (void)applicationWillTerminate:(UIApplication *)application
{
    // Called when the application is about to terminate. Save data if appropriate. See also applicationDidEnterBackground:.
}

/*
// Optional UITabBarControllerDelegate method.
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController
{
}
*/

/*
// Optional UITabBarControllerDelegate method.
- (void)tabBarController:(UITabBarController *)tabBarController didEndCustomizingViewControllers:(NSArray *)viewControllers changed:(BOOL)changed
{
}
*/

@end</pre>

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [iOS学习笔记（5）：标签视图][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/new-project.jpg
 [2]: http://lazynight.me/wp-content/uploads/2012/08/viewtap.jpg
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2369.html