---
title: Asp.Net笔记（3）：Datalist简单入门
author: Flowerowl
layout: post
permalink: /2250.html
duoshuo_thread_id:
  - 2589818
views:
  - 615
bot_views:
  - 6
categories:
  - Asp.Net
  - 代码
---
> DataList 控件，类似于 Repeater 控件，用于显示限制于该控件的项目的重复列表。不过，DataList 控件会默认地在数据项目上添加表格。DataList 控件可被绑定到数据库表、XML 文件或者其他项目列表。

[<img title="3" src="http://lazynight.me/wp-content/uploads/2012/06/3.gif" alt="" width="416" height="520" />][1]

<pre class="lang:default decode:true" title="Default.aspx">&lt;body&gt;
    &lt;form id="form1" runat="server"&gt;
    &lt;div&gt;
        &lt;asp:DataList ID="dtlEmployee" runat="server" BackColor="#000" ForeColor="#ffffff"&gt;
            &lt;ItemTemplate&gt;
                &lt;table&gt;
                    &lt;tr&gt;
                        &lt;td&gt;&lt;b&gt;NO.&lt;/b&gt;&lt;%#Eval("Id") %&gt;&lt;/td&gt;
                    &lt;/tr&gt;
                    &lt;tr&gt;
                        &lt;td&gt;&lt;b&gt;Name:&lt;/b&gt;&lt;%#Eval("Name") %&gt;&lt;/td&gt;
                        &lt;td&gt;&lt;img alt='&lt;%#Eval("Name") %&gt;' src='Image/&lt;%#Eval("Picpath") %&gt;'/&gt;&lt;/td&gt;
                    &lt;/tr&gt;
                    &lt;tr&gt;
                        &lt;td&gt;&lt;b&gt;Description:&lt;/b&gt;&lt;%#Eval("Description") %&gt;&lt;/td&gt;
                    &lt;/tr&gt;
                &lt;/table&gt;
            &lt;/ItemTemplate&gt;
        &lt;/asp:DataList&gt;
    &lt;/div&gt;
    &lt;/form&gt;
&lt;/body&gt;</pre>

&nbsp;

<pre class="lang:default decode:true" title="Default.aspx.cs">using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

public partial class _Default : System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {
        Employee emp = new Employee(1,"Lazynight","z.gif","夜阑，声如窃，独难解。");
        Employee emp2 = new Employee(2, "Flowerowl", "z.gif", "夜阑，声如窃，独难解。");
        Employee emp3 = new Employee(3, "Sinking Ship", "z.gif", "夜阑，声如窃，独难解。");
        List&lt;Employee&gt; lst = new List&lt;Employee&gt;();
        lst.Add(emp);
        lst.Add(emp2);
        lst.Add(emp3);
        this.dtlEmployee.DataSource = lst;
        this.dtlEmployee.DataBind();
    }
}</pre>

&nbsp;

<pre class="lang:default decode:true" title="Employee.cs">public class Employee
{
    private int _id;
    private string _name;
    private string _picpath;

    public string Picpath
    {
        get { return _picpath; }
        set { _picpath = value; }
    }
    private string _description;

    public string Description
    {
        get { return _description; }
        set { _description = value; }
    }

    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }

    public int Id
    {
        get { return _id; }
        set { _id = value; }
    }
    public Employee()
    {
        //
        //TODO: 在此处添加构造函数逻辑
        //
    }

    /// &lt;summary&gt;
    /// 职工类
    /// &lt;/summary&gt;
    /// &lt;param name="id"&gt;职工号&lt;/param&gt;
    /// &lt;param name="name"&gt;职工姓名&lt;/param&gt;
    /// &lt;param name="description"&gt;个人描述&lt;/param&gt;
    public Employee(int id, string name, string picpath,string description)
    {
        this.Id = id;
        this.Name = name;
        this.Picpath = picpath;
        this.Description = description;
    }
}</pre>

&nbsp;

&nbsp;

&nbsp;

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [Asp.Net笔记（3）：Datalist简单入门][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/3.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2250.html