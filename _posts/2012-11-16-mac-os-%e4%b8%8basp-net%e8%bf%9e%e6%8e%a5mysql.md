---
title: Mac OS 下Asp.net连接Mysql
author: Flowerowl
layout: post
permalink: /2612.html
duoshuo_thread_id:
  - 1220743779864322408
views:
  - 1114
bot_views:
  - 2
categories:
  - Asp.Net
  - Mysql
  - 技术杂谈
tags:
  - Mono
---
我的联想笔记本坏了，不能联网，只能用mac写C#了，还没用mono正式做过东西，这次试试吧。

SQLserver没有mac版，mysql搭配Asp.net貌似也不错。

首先，需要下载Connector/Net，地址：http://dev.mysql.com/downloads/connector/net/5.2.html

2，安装Mysql.data.dll到mono framework

su命令行下，sh-3.2# gacutil -i /z/mysql.data.dll  
Installed /z/mysql.data.dll into the gac (/Library/Frameworks/Mono.framework/Versions/2.10.9/lib/mono/gac)

（此dll为下载的文件）

3，创建文件，插入代码：

<pre class="lang:default decode:true ">&lt;%@ Page Language="C#" %&gt;
&lt;%@ Import Namespace="System.Data" %&gt;
&lt;%@ Import Namespace="MySql.Data.MySqlClient" %&gt;
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en"&gt;
  &lt;head&gt;
    &lt;title&gt;CD cat&lt;/title&gt;
    &lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8" /&gt;

    &lt;script runat="server"&gt;
    private void Page_Load(Object sender, EventArgs e)
    {
       string connectionString = "Server=localhost;Database=mono;User ID=root;Pooling=false;";
       MySqlConnection dbcon = new MySqlConnection(connectionString);
       dbcon.Open();

       MySqlDataAdapter adapter = new MySqlDataAdapter("SELECT * FROM artist", dbcon);
       DataSet ds = new DataSet();
       adapter.Fill(ds, "result");

       dbcon.Close();
       dbcon = null;

       ArtistsControl.DataSource = ds.Tables["result"];
       ArtistsControl.DataBind();
    }
    &lt;/script&gt;

  &lt;/head&gt;

  &lt;body&gt;
    &lt;h1&gt;Artists&lt;/h1&gt;
    &lt;asp:DataGrid runat="server" id="ArtistsControl" /&gt;
  &lt;/body&gt;

&lt;/html&gt;</pre>

4,在cs文件中引入mysql.data命名空间

5，测试：

[<img class="alignnone size-full wp-image-2613" title="5ABC0B38-C93B-47A1-9784-0AA34A5E52E7" src="http://lazynight.me/wp-content/uploads/2012/11/5ABC0B38-C93B-47A1-9784-0AA34A5E52E7.jpg" alt="" width="236" height="147" />][1]

转载请注明：[于哲的博客][2] &raquo; [Mac OS 下Asp.net连接Mysql][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/11/5ABC0B38-C93B-47A1-9784-0AA34A5E52E7.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2612.html