---
title: Asp.Net（6）：读取RSS
author: Flowerowl
layout: post
permalink: /2276.html
duoshuo_thread_id:
  - 2769198
views:
  - 804
bot_views:
  - 2
categories:
  - Asp.Net
  - 代码
---
昨天一网友问我讨论Java的RSS订阅器实现，我说没弄过RSS，今天翻了点资料，写写。

在Asp.Net下很容易就实现了读取RSS功能，使用的依然是dll，依然是拖，依然是没技术含量的活儿。

昨天也和另一个网友谈到，说.NET入门门槛儿太低，工资比较少。

我觉得也不无道理，但是也不能全信，各有各的发展，总的来说我是不想去搞赚钱很多的底层设计，我甚至只想搞搞表面的东西，鬼知道我是怎么想的。

写个程序领导不看里边怎么设计的，哪怕你弄个能蹦跶的能看得见的没技术含量的东西也比来个展示不出来的功能强，这就是中国软件发展的悲剧吧。

扯多了，来看下RSS吧。

&nbsp;

* * *

首先，下载RssToolkit工具包。

新建网站项目，加入aspx文件，右击工具栏的常规选项，找到“选择项”，选择你的dll，加入到项目中来。

[<img class="alignnone size-full wp-image-2277" title="rss" src="http://lazynight.me/wp-content/uploads/2012/06/rss.gif" alt="" width="322" height="343" />][1]

这时常规选项卡里会出现rssdatasource和rsshyperlink

往页面内拖入一个rssdatasource和一个dataview

具体实现代码：

<pre class="lang:default decode:true ">&lt;%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %&gt;

&lt;%@ Register Assembly="RssToolkit" Namespace="RssToolkit" TagPrefix="cc1" %&gt;

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;

&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head runat="server"&gt;
    &lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;form id="form1" runat="server"&gt;
    &lt;div&gt;

        &lt;%--&lt;asp:DataList ID="DataList1" runat="server" DataSourceID="RssDataSource1"&gt;
            &lt;ItemTemplate&gt;
                title:
                    &lt;asp:Label ID="titleLable" runat="server" Text='&lt;%#Eval("title")%&gt;'/&gt;
                    &lt;br/&gt;
                link:
                    &lt;asp:Label ID="linkLabel" runat="server" Text='&lt;%#Eval("link")%&gt;' /&gt;
                    &lt;br/&gt;
                description:
                    &lt;asp:Label ID="descriptionLabel" runat="server" Text='&lt;%#Eval("description")%&gt;'/&gt;
                    &lt;br/&gt;
                pubDate:
                    &lt;asp:Label ID="pubDateLabel" runat="server" Text='&lt;%#Eval("pubDate")%&gt;'/&gt;
                    &lt;br/&gt;
            &lt;/ItemTemplate&gt;
        &lt;/asp:DataList&gt;--%&gt;
        &lt;asp:TextBox ID="TextBox1" runat="server"&gt;&lt;/asp:TextBox&gt;
        &lt;asp:Button ID="Button1" runat="server" Text="订阅" onclick="Button1_Click" /&gt;
        &lt;asp:GridView ID="GridView1" runat="server" CellPadding="4" ForeColor="#000000" GridLines="None"&gt;
            &lt;FooterStyle BackColor="#5d7b9d" Font-Bold="True" ForeColor="White" /&gt;
            &lt;RowStyle BackColor="#F7F6F3" ForeColor="#333333" /&gt;
            &lt;PagerStyle BackColor="#284775" ForeColor="White" HorizontalAlign="Center" /&gt;
            &lt;SelectedRowStyle BackColor="#E2DED6" Font-Bold="True" ForeColor="White" /&gt;
            &lt;HeaderStyle BackColor="#5D7B9D" Font-Bold="true" ForeColor="White" /&gt;
            &lt;EditRowStyle BackColor="#999999" /&gt;
            &lt;AlternatingRowStyle BackColor="White" ForeColor="#284775" /&gt; 
        &lt;/asp:GridView&gt;
        &lt;cc1:RssDataSource ID="RssDataSource1" runat="server" MaxItems="0" 
            Url="http://feed.feedsky.com/lazynight"&gt;
        &lt;/cc1:RssDataSource&gt;
    &lt;/div&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

后台代码：

<pre class="lang:default decode:true ">using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using RssToolkit;

public partial class _Default : System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {

    }
    protected void Button1_Click(object sender, EventArgs e)
    {
        string URL = TextBox1.Text;

        //创建一个频道
        GenericRssChannel c = GenericRssChannel.LoadChannel(URL);
        //为GridView绑定数据源
        //数据源来自频道中的所有项目
        GridView1.DataSource = c.SelectItems();
        GridView1.DataBind();
    }
}</pre>

效果图：

[<img class="alignnone size-full wp-image-2278" title="rss2" src="http://lazynight.me/wp-content/uploads/2012/06/rss2.gif" alt="" width="973" height="492" />][2]

&nbsp;

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [Asp.Net（6）：读取RSS][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/rss.gif
 [2]: http://lazynight.me/wp-content/uploads/2012/06/rss2.gif
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2276.html