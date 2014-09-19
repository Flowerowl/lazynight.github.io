---
title: WPF Study notes(5):Binding
author: Flowerowl
layout: post
permalink: /1826.html
duoshuo_thread_id:
  - 1267006
views:
  - 700
bot_views:
  - 2
categories:
  - WPF
  - 代码
---
Today,I&#8217;ll show you some ways to binding source in WPF.

<span style="color: #ff4040;">1.&#8221;NO SOURCE&#8221;: DataContext</span>

<img class="aligncenter size-full wp-image-1827" title="Binding Source" src="http://lazynight.me/wp-content/uploads/2012/04/Binding-Source.gif" alt="" width="229" height="138" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:sys="clr-namespace:System;assembly=mscorlib"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        xmlns:local="clr-namespace:Lazy_Setter_Trigger"
        Title="Bingding Source" Width="350" Height="340"&gt;
    &lt;StackPanel Background="#000" &gt;
        &lt;StackPanel.DataContext&gt;
            &lt;local:Student ID="6" Age="20" Name="Lazynight"/&gt;
        &lt;/StackPanel.DataContext&gt;
        &lt;Grid&gt;
            &lt;StackPanel&gt;
                &lt;TextBox Text="{Binding Path=ID}" Margin="5"/&gt;
                &lt;TextBox Text="{Binding Path=Age}" Margin="5"/&gt;
                &lt;TextBox Text="{Binding Path=Name}" Margin="5"/&gt;
            &lt;/StackPanel&gt;
        &lt;/Grid&gt;
    &lt;/StackPanel&gt;
&lt;/Window&gt;</pre>

<pre class="brush:csharp">using System;
using System.Collections.Generic;
using System.Text;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Shapes;

namespace Lazy_Setter_Trigger
{
	public partial class MainWindow : Window
	{
		public MainWindow()
		{
			this.InitializeComponent();
		}
	}
    public class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
    }
}</pre>

<span style="color: #ff4040;">2.&#8221;NO Path&#8221;</span>

<img class="aligncenter size-full wp-image-1829" title="NO PATH" src="http://lazynight.me/wp-content/uploads/2012/04/NO-PATH.gif" alt="" width="304" height="111" />

<pre class="brush:xml">&lt;StackPanel&gt;
        &lt;StackPanel.Resources&gt;
            &lt;sys:String x:Key="lazyString"&gt;
                Hello Everybody!
            &lt;/sys:String&gt;
        &lt;/StackPanel.Resources&gt;
        &lt;TextBlock x:Name="textBlock1" TextWrapping="Wrap" Text="{Binding Path=.,Source={StaticResource ResourceKey=lazyString}}" FontSize="20" Margin="5"/&gt;
        &lt;TextBlock x:Name="textBlock2" TextWrapping="Wrap" Text="{Binding Source={StaticResource ResourceKey=lazyString}}" FontSize="10" Margin="5" /&gt;
    &lt;/StackPanel&gt;</pre>

<span style="color: #ff4040;">3.&#8221;NO PATH NO SOURCE&#8221;</span>

<img class="aligncenter size-full wp-image-1831" title="No path No source" src="http://lazynight.me/wp-content/uploads/2012/04/No-path-No-source.gif" alt="" width="228" height="118" />

<pre class="brush:xml">&lt;StackPanel&gt;
        &lt;StackPanel.DataContext&gt;
            &lt;sys:String&gt;
                Lazynight
            &lt;/sys:String&gt;
        &lt;/StackPanel.DataContext&gt;
        &lt;Grid&gt;
            &lt;StackPanel&gt;
                &lt;TextBlock Text="{Binding}" Margin="5"/&gt;
                &lt;TextBlock Text="{Binding}" Margin="5"/&gt;
                &lt;TextBlock Text="{Binding}" Margin="5"/&gt;
            &lt;/StackPanel&gt;
        &lt;/Grid&gt;
    &lt;/StackPanel&gt;</pre>

<span style="color: #ff4040;">4.ItemsSource</span>

<img class="aligncenter size-full wp-image-1833" title="ItemsSource" src="http://lazynight.me/wp-content/uploads/2012/04/ItemsSource.gif" alt="" width="213" height="240" />

<pre class="brush:xml">&lt;StackPanel x:Name="stackPanel" Background="#000"&gt;
        &lt;TextBlock Text="Student ID:" Foreground="#ff4040" FontWeight="Bold" Margin="5"/&gt;
        &lt;TextBox x:Name="textBoxId" Margin="5"/&gt;
        &lt;TextBlock Text="Student List:" Foreground="#FF4040" FontWeight="Bold" Margin="5"/&gt;
        &lt;ListBox x:Name="listBoxStudents" Height="110" Margin="5"/&gt;
    &lt;/StackPanel&gt;</pre>

<pre class="brush:csharp">namespace Lazy_Setter_Trigger
{
	public partial class MainWindow : Window
	{
		public MainWindow()
		{
			this.InitializeComponent();
            List&lt;Student&gt; stuList = new List&lt;Student&gt;()
            {
                new Student(){ID=0,Age=19,Name="Lazynight"},
                new Student(){ID=1,Age=20,Name="Flowerowl"},
                new Student(){ID=2,Age=21,Name="NightDivides"},
                new Student(){ID=3,Age=22,Name="Sinking ship"}
            };
            this.listBoxStudents.ItemsSource = stuList;
            this.listBoxStudents.DisplayMemberPath = "Name";
            Binding binding = new Binding("SelectedItem.ID") { Source=this.listBoxStudents};
            this.textBoxId.SetBinding(TextBox.TextProperty,binding);
		}
	}
    public class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
    }
}</pre>

<img class="aligncenter size-full wp-image-1836" title="ItemsSource" src="http://lazynight.me/wp-content/uploads/2012/04/ItemsSource1.gif" alt="" width="246" height="210" />

<pre class="brush:xml">&lt;StackPanel x:Name="stackPanel" Background="#000"&gt;
        &lt;TextBlock Text="Student ID:" Foreground="#ff4040" FontWeight="Bold" Margin="5"/&gt;
        &lt;TextBox x:Name="textBoxId" Margin="5"/&gt;
        &lt;TextBlock Text="Student List:" Foreground="#FF4040" FontWeight="Bold" Margin="5"/&gt;
        &lt;ListBox x:Name="listBoxStudents" Height="110" Margin="5"&gt;
            &lt;ListBox.ItemTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;StackPanel Orientation="Horizontal"&gt;
                        &lt;TextBlock Text="{Binding Path=ID}" Width="30"/&gt;
                        &lt;TextBlock Text="{Binding Path=Age}" Width="30"/&gt;
                        &lt;TextBlock Text="{Binding Path=Name}" Width="70"/&gt;
                    &lt;/StackPanel&gt;
                &lt;/DataTemplate&gt;
            &lt;/ListBox.ItemTemplate&gt;
        &lt;/ListBox&gt;
    &lt;/StackPanel&gt;</pre>

<span style="color: #ff4040;"> 5.LINQ</span>

<img class="aligncenter size-full wp-image-1839" title="Linq" src="http://lazynight.me/wp-content/uploads/2012/04/Linq.gif" alt="" width="259" height="122" />

<pre class="brush:xml">&lt;StackPanel x:Name="stackPanel" Background="#000"&gt;
        &lt;ListView x:Name="listViewStudent" Height="150" Margin="5"&gt;
            &lt;ListView.View&gt;
                &lt;GridView&gt;
                    &lt;GridViewColumn Header="ID" Width="60" DisplayMemberBinding="{Binding ID}"/&gt;
                    &lt;GridViewColumn Header="Age" Width="60" DisplayMemberBinding="{Binding Age}"/&gt;
                    &lt;GridViewColumn Header="Name" Width="60" DisplayMemberBinding="{Binding Name}"/&gt;
                &lt;/GridView&gt;
            &lt;/ListView.View&gt;
        &lt;/ListView&gt;
    &lt;/StackPanel&gt;</pre>

&nbsp;

<pre class="brush:csharp">using System.Linq;
namespace Lazy_Setter_Trigger
{
	public partial class MainWindow : Window
	{
		public MainWindow()
		{
			this.InitializeComponent();
            List&lt;Student&gt; stuList = new List&lt;Student&gt;() 
            {
                new Student(){ID=0,Age=19,Name="Lazynight"},
                new Student(){ID=1,Age=20,Name="Flowerowl"},
                new Student(){ID=2,Age=21,Name="NightDivides"},
                new Student(){ID=3,Age=22,Name="Sinking ship"}
            };
            this.listViewStudent.ItemsSource = from stu in stuList where stu.Name.StartsWith("L") select stu; 
		}
	}
    public class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Age { get; set; }
    }
}</pre>

<span style="color: #ff4040;"> 6.ADO.NET</span>

<span style="color: #ff4040;">7.XML</span>

<img class="aligncenter size-full wp-image-1841" title="xml" src="http://lazynight.me/wp-content/uploads/2012/04/xml.gif" alt="" width="334" height="229" />

<pre class="brush:xml">&lt;StackPanel x:Name="stackPanel" Background="#000"&gt;
        &lt;ListView x:Name="listViewStudent" Height="150" Margin="5"&gt;
            &lt;ListView.View&gt;
                &lt;GridView&gt;
                    &lt;GridViewColumn Header="ID" Width="60" DisplayMemberBinding="{Binding XPath=@ID}"/&gt;
                    &lt;GridViewColumn Header="Age" Width="60" DisplayMemberBinding="{Binding XPath=Age}"/&gt;
                    &lt;GridViewColumn Header="Name" Width="60" DisplayMemberBinding="{Binding XPath=Name}"/&gt;
                &lt;/GridView&gt;
            &lt;/ListView.View&gt;
        &lt;/ListView&gt;
        &lt;Button Content="Load" Click="Button_Click" Height="25" Margin="5"/&gt;
    &lt;/StackPanel&gt;</pre>

<pre class="brush:xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;StudentList&gt;
	&lt;Student ID="1"&gt;
		&lt;Name&gt;Lazynight&lt;/Name&gt;
	&lt;/Student&gt;
	&lt;Student ID="2"&gt;
		&lt;Name&gt;Flowerowl&lt;/Name&gt;
	&lt;/Student&gt;
	&lt;Student ID="3"&gt;
		&lt;Name&gt;Frozen World&lt;/Name&gt;
	&lt;/Student&gt;
	&lt;Student ID="4"&gt;
		&lt;Name&gt;Sinking Ship&lt;/Name&gt;
	&lt;/Student&gt;
	&lt;Student ID="5"&gt;
		&lt;Name&gt;Orange&lt;/Name&gt;
	&lt;/Student&gt;
&lt;/StudentList&gt;</pre>

<pre class="brush:csharp">private void Button_Click(object sender, RoutedEventArgs e)
        {
            XmlDataDocument doc = new XmlDataDocument();
            doc.Load(AppDomain.CurrentDomain.BaseDirectory+"Data.xml");
            XmlDataProvider xdp = new XmlDataProvider();
            xdp.Document = doc;
            xdp.XPath = @"/StudentList/Student";
            this.listViewStudent.DataContext = xdp;
            this.listViewStudent.SetBinding(ListView.ItemsSourceProperty,new Binding());
        }</pre>

&nbsp;

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [WPF Study notes(5):Binding][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1826.html