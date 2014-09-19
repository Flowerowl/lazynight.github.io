---
title: Asp.Net（5）：使用全球通Gravatar头像
author: Flowerowl
layout: post
permalink: /2267.html
duoshuo_thread_id:
  - 2746015
views:
  - 791
bot_views:
  - 6
categories:
  - Asp.Net
  - 代码
---
在WP中留言大多数网友都会有一个Gravatar头像，方便使用头像。

今天忽然想起来，用Asp.Net折腾一下，来吧。

扫盲：<span style="color: #ff0000;"><a href="http://baike.baidu.com/view/675247.htm" target="_blank"><span style="color: #ff0000;">关于Gravatar</span></a></span>

先下载控件动态链接库： <span style="color: #ff0000;"><a href="http://www.freshclickmedia.com/wp-content/uploads/2008/02/gravatar.zip" target="_blank"><span style="color: #ff0000;">下载</span></a></span>

新建web应用程序，引入下载的库文件。

在页首加入如下代码：

<span style="color: #ff0000;"><%@ Page Language=&#8221;C#&#8221; AutoEventWireup=&#8221;true&#8221; CodeBehind=&#8221;Default.aspx.cs&#8221; Inherits=&#8221;Gavatar.Default&#8221; %></span>  
<span style="color: #ff0000;"><%@ Register Assembly=&#8221;FreshClickmedia.Web&#8221; Namespace=&#8221;FreshClickMedia.Web.UI.WebControls&#8221; TagPrefix=&#8221;fcm&#8221; %></span>

<pre class="lang:default decode:true" title="Default.aspx">&lt;%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="Gavatar.Default" %&gt;
&lt;%@ Register Assembly="FreshClickmedia.Web" Namespace="FreshClickMedia.Web.UI.WebControls" TagPrefix="fcm" %&gt;
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;

&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head runat="server"&gt;
    &lt;title&gt;&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;form id="form1" runat="server"&gt;
    &lt;div&gt;
        &lt;asp:Image ID="Image" runat="server" /&gt;
        &lt;asp:TextBox ID="Email" runat="server"&gt;&lt;/asp:TextBox&gt;
        &lt;asp:Button ID="Button1" runat="server" Text="提交" onclick="Button1_Click" /&gt;
    &lt;/div&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

后台代码：

<pre class="lang:default decode:true">using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using FreshClickMedia.Web.UI.WebControls;
using System.Web.Security;
namespace Gavatar
{
    public partial class Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
            if (!IsPostBack)
            {  

            }
        }
        protected void Button1_Click(object sender, EventArgs e)
        {

            string email = this.Email.Text;
            string authen = FormsAuthentication.HashPasswordForStoringInConfigFile(email,"MD5");
            string imageurl = "http://www.gravatar.com/avatar.php?gravatar_id=" + authen + "&rating=G&size=80&default=http://www.silverlight.net/content/general/events/scottgu.jpg";
            Image.ImageUrl = imageurl.ToLower();//注意，加密后后默认为大写，，需要转成小写
        }
    }
}</pre>

效果图：

[<img class="alignnone size-full wp-image-2269" title="Gravatar" src="http://lazynight.me/wp-content/uploads/2012/06/Gravatar.gif" alt="" width="337" height="134" />][1]

<span style="color: #ff0000;">gravatar_id：Email地址的MD5</span>

<span style="color: #ff0000;">rating：允许头像的级别</span>

<span style="color: #ff0000;">size：头像的大小</span>

<span style="color: #ff0000;">default：默认头像的URL</span>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [Asp.Net（5）：使用全球通Gravatar头像][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/Gravatar.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2267.html