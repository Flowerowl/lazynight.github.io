---
title: iOS学习笔记（8）：表格视图
author: Flowerowl
layout: post
permalink: /2380.html
duoshuo_thread_id:
  - 6209199
views:
  - 1065
bot_views:
  - 2
categories:
  - Object-C
  - 代码
tags:
  - ios
---
<span style="color: #ff0000;"><a href="http://dl.dbank.com/c0dpuf9vop" target="_blank"><span style="color: #ff0000;">下载源码</span></a></span>

[<img class="alignnone size-full wp-image-2381" title="tablebview" src="http://lazynight.me/wp-content/uploads/2012/08/tablebview.jpg" alt="" width="318" height="481" />][1]

&nbsp;

&nbsp;

&nbsp;

<pre class="lang:default decode:true ">//
//  XTableViewController.m
//  TableViewDemo
//
//  Created by 哲   于 on 12-8-13.
//  Copyright (c) 2012年 哈尔滨理工大学. All rights reserved.
//

#import "XTableViewController.h"

@interface XTableViewController ()

@end

@implementation XTableViewController

- (id)initWithStyle:(UITableViewStyle)style
{
    self = [super initWithStyle:style];
    if (self) {
        // Custom initialization
    }
    return self;
}

- (void)viewDidLoad
{
    [super viewDidLoad];
     self.tableView.rowHeight=150.;

    // Uncomment the following line to preserve selection between presentations.
    // self.clearsSelectionOnViewWillAppear = NO;

    // Uncomment the following line to display an Edit button in the navigation bar for this view controller.
    // self.navigationItem.rightBarButtonItem = self.editButtonItem;

}

- (void)viewDidUnload
{
    [super viewDidUnload];
    // Release any retained subviews of the main view.
    // e.g. self.myOutlet = nil;

}

- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation
{
    return (interfaceOrientation == UIInterfaceOrientationPortrait);
}

#pragma mark - Table view data source

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
#warning Potentially incomplete method implementation.
    // Return the number of sections.
    return 20;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    // Return the number of rows in the section.
    return 2;
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *CellIdentifier = @"Cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];

    if(!cell){
        cell=[[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:CellIdentifier]autorelease];
    }
    if(indexPath.row%2==1){
        cell.textLabel.textColor=[UIColor redColor];
    }else{
        cell.textLabel.textColor=[UIColor blueColor];
    }
    cell.textLabel.text=[NSString stringWithFormat:@"第%i行", indexPath.row];
    cell.detailTextLabel.text=@"Lazynight";
    cell.imageView.image=[UIImage imageNamed:@"ibook"];
    // Configure the cell...
    return cell;

}

- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section{
    return [NSString stringWithFormat:@"第%d小节",section];
}
//- (void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath{
//    cell.backgroundColor=[UIColor orangeColor];//更改表格背景颜色
//}

/*
// Override to support conditional editing of the table view.
- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath
{
    // Return NO if you do not want the specified item to be editable.
    return YES;
}
*/

/*
// Override to support editing the table view.
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
{
    if (editingStyle == UITableViewCellEditingStyleDelete) {
        // Delete the row from the data source
        [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath] withRowAnimation:UITableViewRowAnimationFade];
    }   
    else if (editingStyle == UITableViewCellEditingStyleInsert) {
        // Create a new instance of the appropriate class, insert it into the array, and add a new row to the table view
    }   
}
*/

/*
// Override to support rearranging the table view.
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)fromIndexPath toIndexPath:(NSIndexPath *)toIndexPath
{
}
*/

/*
// Override to support conditional rearranging of the table view.
- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath
{
    // Return NO if you do not want the item to be re-orderable.
    return YES;
}
*/

#pragma mark - Table view delegate

- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    // Navigation logic may go here. Create and push another view controller.
    /*
     &lt;#DetailViewController#&gt; *detailViewController = [[&lt;#DetailViewController#&gt; alloc] initWithNibName:@"&lt;#Nib name#&gt;" bundle:nil];
     // ...
     // Pass the selected object to the new view controller.
     [self.navigationController pushViewController:detailViewController animated:YES];
     [detailViewController release];
     */
    UIAlertView *alert=[[[UIAlertView alloc] initWithTitle:@"选中了" message:[NSString stringWithFormat:@"您选中了第%d行",indexPath.row] delegate:nil cancelButtonTitle:@"OK" otherButtonTitles:nil] autorelease];
    [alert show];

}

@end</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [iOS学习笔记（8）：表格视图][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/tablebview.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2380.html