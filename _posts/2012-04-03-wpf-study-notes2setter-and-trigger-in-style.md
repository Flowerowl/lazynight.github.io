---
title: WPF Study notes(2):Setter and Trigger in Style
author: Flowerowl
layout: post
permalink: /1771.html
duoshuo_thread_id:
  - 1267002
views:
  - 798
bot_views:
  - 1
categories:
  - WPF
  - 代码
---
This article is about **<span style="color: #ff4040;">Style</span>** in **WPF**.

As the **CSS** in **HTML**,Style controlls static appearance and behavior of our WPF applications.

Two of the most important elements in Style are <span style="color: #ff4040;"><strong>Setter</strong></span> and<span style="color: #ff4040;"><strong> Trigger</strong></span>.

There are five kinds of Trigger,I&#8217;ll show you two of them first. **<span style="color: #ff4040;">Basic Trigger</span>** and **<span style="color: #ff4040;">MultiTrigger</span>**.

Well,Let us see how it works.

<span style="color: #ff4040;">Demo:</span>

<img class="aligncenter size-full wp-image-1772" title="夜阑" src="http://lazynight.me/wp-content/uploads/2012/04/Lazy_WPF.gif" alt="" width="526" height="349" />

<span style="color: #ff4040;">XAML:</span>

<pre class="brush:applescript">&lt;Window
	xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	x:Class="Lazy_Setter_Trigger.MainWindow"
	x:Name="Window"
	Title="Hello WPF!"
	Width="640" Height="480"&gt;
	&lt;Window.Resources&gt;
		&lt;Style TargetType="TextBlock"&gt;
			&lt;Style.Setters&gt;
				&lt;Setter Property="FontSize" Value="40"/&gt;
				&lt;Setter Property="TextDecorations" Value="Underline"/&gt;
				&lt;Setter Property="FontStyle" Value="Italic"/&gt;
			&lt;/Style.Setters&gt;
		&lt;/Style&gt;
		&lt;Style TargetType="CheckBox"&gt;
			&lt;Style.Triggers&gt;
				&lt;Trigger Property="IsChecked" Value="true"&gt;
					&lt;Trigger.Setters&gt;&lt;!--此标签可以省略--&gt;
						&lt;Setter Property="FontSize" Value="20"/&gt;
						&lt;Setter Property="Foreground" Value="Orange"/&gt;
					&lt;/Trigger.Setters&gt;
				&lt;/Trigger&gt;
				&lt;MultiTrigger&gt;&lt;!--MultiTrigger,必须同时多个条件同时成立才会被出发--&gt;
					&lt;MultiTrigger.Conditions&gt;
						&lt;Condition Property="IsChecked" Value="true"/&gt;
						&lt;Condition Property="Content" Value="Twenty Years"/&gt;
					&lt;/MultiTrigger.Conditions&gt;
					&lt;MultiTrigger.Setters&gt;
						&lt;Setter Property="FontSize" Value="30"/&gt;
						&lt;Setter Property="Foreground" Value="#FFFF4040"/&gt;
					&lt;/MultiTrigger.Setters&gt;
				&lt;/MultiTrigger&gt;
			&lt;/Style.Triggers&gt;
		&lt;/Style&gt;
	&lt;/Window.Resources&gt;
	&lt;StackPanel Margin="5"&gt;
		&lt;TextBlock Text="Hello WPF!"/&gt;
		&lt;TextBlock Text="This is a sample for Style."/&gt;
		&lt;TextBlock Text="by Lazynight 2012.4.3" Style="{x:Null}"/&gt;
		&lt;Line Margin="0 10 0 10" Stroke="Black" X1="0" Y1="0" X2="640" Y2="0" Stretch="Fill"/&gt;
		&lt;CheckBox Content="Lazynight" Margin="5"/&gt;
		&lt;CheckBox Content="Flowerowl" Margin="10 20 300 5"/&gt;&lt;!--Maring 和CSS的方向顺序不一样，此处Margin顺序为：左，上，右，下--&gt;
		&lt;CheckBox Content="NightDivides" Margin="5"/&gt;
		&lt;CheckBox Content="Twenty Years" /&gt;
	&lt;/StackPanel&gt;
&lt;/Window&gt;</pre>

We know that WPF is driven by data, different from those languages driven by event.  
OK,there are still three triggers: **<span style="color: #ff4040;">DataTrigger</span>**,**<span style="color: #ff4040;">MultiDataTrigger</span>**,**<span style="color: #ff4040;">EventTrigger</span>**,I&#8217;ll show you one by one.

<span style="color: #ff4040;">1.DataTrigger Demo:</span>

When textBox&#8217;s length is greater than 6 , it&#8217;s border will be changed.

<img class="aligncenter size-full wp-image-1783" title="DataTtrigger" src="http://lazynight.me/wp-content/uploads/2012/04/DataTtrigger.gif" alt="" width="327" height="149" />

<span style="color: #ff4040;">XAML:</span>

<pre class="brush:xml">&lt;Window
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="clr-namespace:Lazy_Setter_Trigger"
    x:Class="Lazy_Setter_Trigger.MainWindow"
	Title="Hello WPF!"
	x:Name="Window"
	Width="640" Height="480"&gt;
    &lt;Window.Resources&gt;
        &lt;local:L2BConvert x:Key="cvtr"/&gt;
        &lt;Style TargetType="TextBox"&gt;
            &lt;Style.Triggers&gt;
                &lt;DataTrigger Binding="{Binding RelativeSource={x:Static RelativeSource.Self},Path=Text.Length,Converter={StaticResource cvtr}}" Value="true"&gt;
                    &lt;Setter Property="BorderBrush" Value="#ff4040"/&gt;
                    &lt;Setter Property="BorderThickness" Value="3" /&gt;
                &lt;/DataTrigger&gt;
            &lt;/Style.Triggers&gt;
        &lt;/Style&gt;
    &lt;/Window.Resources&gt;
    &lt;StackPanel&gt;
        &lt;TextBox Margin="5" /&gt;
        &lt;TextBox Margin="5" /&gt;
        &lt;TextBox Margin="5"/&gt;
    &lt;/StackPanel&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">CS:</span>

<pre class="brush:csharp">public class L2BConvert:IValueConverter
    {
        public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
        {
            int textLength = (int)value;
            return textLength &gt; 6 ? true : false;
        }
        public object ConvertBack(object value,Type targetType,object parameter,CultureInfo culture)
        {
            throw new NotImplementedException();
        }
    }</pre>

<span style="color: #ff4040;">2.MultiDataTrigger Demo:</span>

<img class="aligncenter size-full wp-image-1784" title="MultiDataTrigger" src="http://lazynight.me/wp-content/uploads/2012/04/MultiDataTrigger.gif" alt="" width="400" height="132" />

<span style="color: #ff4040;">XAML:</span>

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="MultiDataTrigger" Width="400" Height="300"&gt;
    &lt;Window.Resources&gt;
        &lt;Style TargetType="ListBoxItem"&gt;
            &lt;!--使用Style设置DataTemplate--&gt;
            &lt;Setter Property="ContentTemplate"&gt;
                &lt;Setter.Value&gt;
                    &lt;DataTemplate&gt;
                        &lt;StackPanel Orientation="Horizontal"&gt;
                            &lt;TextBlock Text="{Binding ID}" Width="60"/&gt;
                            &lt;TextBlock Text="{Binding Age}" Width="60"/&gt;
                            &lt;TextBlock Text="{Binding Name}" Width="120"/&gt;
                        &lt;/StackPanel&gt;
                    &lt;/DataTemplate&gt;
                &lt;/Setter.Value&gt;
            &lt;/Setter&gt;
            &lt;!--MultiDataTrigger--&gt;
            &lt;Style.Triggers&gt;
                &lt;MultiDataTrigger&gt;
                    &lt;MultiDataTrigger.Conditions&gt;
                        &lt;Condition Binding="{Binding Path=ID}" Value="100"/&gt;
                        &lt;Condition Binding="{Binding Path=Name}" Value="Lazynight"/&gt;
                    &lt;/MultiDataTrigger.Conditions&gt;
                    &lt;MultiDataTrigger.Setters&gt;
                        &lt;Setter Property="Background" Value="#000" /&gt;
                        &lt;Setter Property="Foreground" Value="#fff"/&gt;
                    &lt;/MultiDataTrigger.Setters&gt;
                &lt;/MultiDataTrigger&gt;
            &lt;/Style.Triggers&gt;
        &lt;/Style&gt;
    &lt;/Window.Resources&gt;
    &lt;StackPanel&gt;
        &lt;ListBox x:Name="listBoxStudent" Margin="5"/&gt;
    &lt;/StackPanel&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">CS:</span>

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
using System.Globalization;

namespace Lazy_Setter_Trigger
{
	public partial class MainWindow : Window
	{
		public MainWindow()
		{
			this.InitializeComponent();
            List&lt;Student&gt; student = new List&lt;Student&gt;()
            {
                new Student(100,20,"Lazynight"),
                new Student(101,19,"Flowerowl"),
                new Student(102,18,"Sinking Ship"),
                new Student(103,17,"Frozen World"),
                new Student(104,21,"Dead Meat")
            };
            this.listBoxStudent.ItemsSource = student;
		}
	}

    public class Student
    {
        public int ID { get; set; }
        public int Age { get; set; }
        public string Name { get; set; }
        public Student(int id,int age,string name)
        {
            ID = id;
            Age = age;
            Name = name;
        }
    }
}</pre>

<span style="color: #ff4040;">3.EventTrigger</span>

This is the most special one.It&#8217;s not driven by data neither the property ,it&#8217;s driven by event.

And ,after being triggered，it&#8217;s not use &#8220;Setter&#8221; ，but to run a period of animation。

So,the animation of UI is always connected to EventTrigger.

<span style="color: #ff4040;">Demo:</span>

<img class="aligncenter size-full wp-image-1785" title="EventTrigger1" src="http://lazynight.me/wp-content/uploads/2012/04/EventTrigger1.gif" alt="" width="230" height="104" /><img class="aligncenter size-full wp-image-1786" title="EventTrigger2" src="http://lazynight.me/wp-content/uploads/2012/04/EventTrigger2.gif" alt="" width="219" height="198" />

<span style="color: #ff4040;">XAML:</span>

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="EventTrigger" Width="400" Height="300"&gt;
    &lt;Window.Resources&gt;
        &lt;Style TargetType="Button"&gt;
            &lt;Style.Triggers&gt;
                &lt;!--鼠标进入--&gt;
                &lt;EventTrigger RoutedEvent="MouseEnter"&gt;
                    &lt;BeginStoryboard&gt;
                        &lt;Storyboard&gt;
                            &lt;DoubleAnimation To="150" Duration="0:0:0.2" Storyboard.TargetProperty="Width"/&gt;
                            &lt;DoubleAnimation To="150" Duration="0:0:0.2" Storyboard.TargetProperty="Height"/&gt;
                        &lt;/Storyboard&gt;
                    &lt;/BeginStoryboard&gt;
                &lt;/EventTrigger&gt;
                &lt;!--鼠标离开--&gt;
                &lt;EventTrigger RoutedEvent="MouseLeave"&gt;
                    &lt;BeginStoryboard&gt;
                        &lt;Storyboard&gt;
                            &lt;DoubleAnimation Duration="0:0:0.2" Storyboard.TargetProperty="Width"/&gt;
                            &lt;DoubleAnimation Duration="0:0:0.2" Storyboard.TargetProperty="Height"/&gt;
                        &lt;/Storyboard&gt;
                    &lt;/BeginStoryboard&gt;
                &lt;/EventTrigger&gt;
            &lt;/Style.Triggers&gt;
        &lt;/Style&gt;
    &lt;/Window.Resources&gt;
    &lt;Canvas&gt;
        &lt;Button Width="40" Height="40" Content="GO!"/&gt;
    &lt;/Canvas&gt;
&lt;/Window&gt;</pre>

PS:用鸟语写不是为了装13，只是为了提高鸟语水平，以便以后做个鸟人，方便看鸟书。  
**<span style="color: #ff4040;"><a href="http://dl.dbank.com/c0ztt2aw4n" target="_blank"><span style="color: #ff4040;">Download</span></a></span>**

转载请注明：[于哲的博客][1] &raquo; [WPF Study notes(2):Setter and Trigger in Style][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1771.html