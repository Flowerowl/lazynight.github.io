---
title: WPF Study notes(4):Animation
author: Flowerowl
layout: post
permalink: /1810.html
duoshuo_thread_id:
  - 1267004
views:
  - 730
bot_views:
  - 3
categories:
  - WPF
  - 代码
---
I was confused about the animation in Visual Studio 2011 Beta&#8217;s installation and Metro&#8217;s applications.

There is no doubt that their  interface is perfect.

Demo:

This pic can&#8217;t show the path which these buttons stand on,please run code yourself.

<img class="aligncenter size-full wp-image-1817" title="Animation" src="http://lazynight.me/wp-content/uploads/2012/04/Animation.gif" alt="" width="250" height="185" />

<span style="color: #ff4040;">XAML:</span>

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Animation" Width="350" Height="340"&gt;
    &lt;Grid x:Name="LayoutRoot"&gt;
        &lt;Grid.Resources&gt;
            &lt;!--Moving Path--&gt;
            &lt;PathGeometry x:Key="movingPath" Figures="M 0,150 C 300,100 300,400 600,120"/&gt;
        &lt;/Grid.Resources&gt;
        &lt;!--Layout--&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="70"/&gt;
            &lt;ColumnDefinition Width="70"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;Grid.RowDefinitions&gt;
            &lt;RowDefinition Height="70"/&gt;
            &lt;RowDefinition Height="70"/&gt;
        &lt;/Grid.RowDefinitions&gt;
        &lt;!--Translation--&gt;
        &lt;Button Content="Translation" HorizontalAlignment="Left" VerticalAlignment="Top" Width="70" Height="70" Click="Button_Click" Grid.Row="0" Grid.Column="0"&gt;
            &lt;Button.RenderTransform&gt;
                &lt;TranslateTransform x:Name="doubleAnimation" /&gt;
            &lt;/Button.RenderTransform&gt;
        &lt;/Button&gt;
        &lt;!--Bounce--&gt;
        &lt;Button Content="Bounce" Width="70" Height="70" Click="Button_Click_1"  VerticalAlignment="Top" Grid.Row="0" Grid.Column="1"&gt;
            &lt;Button.RenderTransform&gt;
                &lt;TranslateTransform x:Name="bounce"/&gt;
            &lt;/Button.RenderTransform&gt;
        &lt;/Button&gt;
        &lt;!--Path Animation--&gt;
        &lt;Button Content="Path Animation" Width="70" Height="70" Click="Button_Click_2" Grid.Row="1" Grid.Column="0"&gt;
            &lt;Button.RenderTransform&gt;
                &lt;TranslateTransform x:Name="render"/&gt;
            &lt;/Button.RenderTransform&gt;
        &lt;/Button&gt;
        &lt;!--Key Frames--&gt;
        &lt;Button Content="Key Frame" Width="70" Height="70" Click="Button_Click_3" Grid.Row="1" Grid.Column="1"&gt;
            &lt;Button.RenderTransform&gt;
                &lt;TranslateTransform x:Name="keyframe"/&gt;
            &lt;/Button.RenderTransform&gt;
        &lt;/Button&gt;
    &lt;/Grid&gt;
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
using System.Windows.Media.Animation;
using System.Windows.Shapes;

namespace Lazy_Setter_Trigger
{
	public partial class MainWindow : Window
	{
		public MainWindow()
		{
			this.InitializeComponent();
		}

        private void Button_Click(object sender, RoutedEventArgs e)
        {
            DoubleAnimation x = new DoubleAnimation();
            DoubleAnimation y = new DoubleAnimation();
            //指定起点
            // x.From = 0D;
            //y.From = 0D;
            //指定终点
            Random r = new Random();
            x.To = r.NextDouble() * 300;
            y.To = r.NextDouble() * 300;
            x.AccelerationRatio = 1;//加速速率
            y.AccelerationRatio = 0.5;
            //x.RepeatBehavior = RepeatBehavior.Forever;//动画重复循环

            //指定时长
            Duration duration = new Duration(TimeSpan.FromMilliseconds(300));
            x.Duration = duration;
            y.Duration = duration;
            this.doubleAnimation.BeginAnimation(TranslateTransform.XProperty,x);
            this.doubleAnimation.BeginAnimation(TranslateTransform.YProperty,y);
        }

        private void Button_Click_1(object sender, RoutedEventArgs e)
        {
            DoubleAnimation x = new DoubleAnimation();
            DoubleAnimation y = new DoubleAnimation();
            //设置反弹
            BounceEase be = new BounceEase();
            be.Bounces = 7;//弹跳三次
            be.Bounciness = 2;//弹性程度，越大越低
            y.EasingFunction = be;
            x.To = 300;
            y.To = 300;
            //指定时长
            Duration duration = new Duration(TimeSpan.FromMilliseconds(2000));
            x.Duration = duration;
            y.Duration = duration;
            this.bounce.BeginAnimation(TranslateTransform.XProperty,x);
            this.bounce.BeginAnimation(TranslateTransform.YProperty,y);
        }

        private void Button_Click_2(object sender, RoutedEventArgs e)
        {
            //从XAML代码中获取移动路径
            PathGeometry pg = this.LayoutRoot.FindResource("movingPath") as PathGeometry;
            Duration duration = new Duration(TimeSpan.FromMilliseconds(600));
            //创建动画
            DoubleAnimationUsingPath x = new DoubleAnimationUsingPath();
            x.PathGeometry = pg;
            x.Source = PathAnimationSource.X;
            x.Duration = duration;

            DoubleAnimationUsingPath y = new DoubleAnimationUsingPath();
            y.PathGeometry = pg;
            y.Source = PathAnimationSource.Y;
            y.Duration = duration;
            //执行动画
            this.render.BeginAnimation(TranslateTransform.XProperty,x);
            this.render.BeginAnimation(TranslateTransform.YProperty,y);
        }

        private void Button_Click_3(object sender, RoutedEventArgs e)
        {   //创建动画
            DoubleAnimationUsingKeyFrames x = new DoubleAnimationUsingKeyFrames();
            x.Duration = new Duration(TimeSpan.FromMilliseconds(1000));
            //创建，添加关键帧
            SplineDoubleKeyFrame kf = new SplineDoubleKeyFrame();
            kf.KeyTime = KeyTime.FromPercent(1);
            kf.Value=400;
            KeySpline ks = new KeySpline();
            ks.ControlPoint1 = new Point(0,1);
            ks.ControlPoint2 = new Point(1,0);
            kf.KeySpline = ks;
            x.KeyFrames.Add(kf);
            this.keyframe.BeginAnimation(TranslateTransform.XProperty,x);
        }
	}
}</pre>

转载请注明：[于哲的博客][1] &raquo; [WPF Study notes(4):Animation][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1810.html