---
title: WPF Study notes(3):Painting
author: Flowerowl
layout: post
permalink: /1789.html
duoshuo_thread_id:
  - 1267003
views:
  - 704
bot_views:
  - 2
categories:
  - WPF
  - 代码
---
This article is about painting and animation in WPF.

<span style="color: #ff4040;"><strong>Painting:</strong></span>

<span style="color: #ff4040;">1.Line</span>

Demo:  
<img class="aligncenter size-full wp-image-1791" title="Lines" src="http://lazynight.me/wp-content/uploads/2012/04/Lines.gif" alt="" width="285" height="138" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Lines" Width="292" Height="275"&gt;
    &lt;Grid&gt;
        &lt;Line X1="10" Y1="20" X2="260" Y2="20" Stroke="Red" StrokeThickness="10"/&gt;
        &lt;Line X1="10" Y1="40" X2="260" Y2="40" Stroke="Orange" StrokeDashArray="3" StrokeThickness="3"/&gt;
        &lt;Line X1="10" Y1="60" X2="260" Y2="60" Stroke="Black" StrokeEndLineCap="Triangle" StrokeThickness="10"/&gt;
        &lt;Line X1="10" Y1="80" X2="260" Y2="80" StrokeEndLineCap="Round" StrokeThickness="10"&gt;
            &lt;Line.Stroke&gt;
                &lt;LinearGradientBrush EndPoint="0,0.5" StartPoint="0.5,0.5"&gt;
                    &lt;GradientStop Color="Green"/&gt;
                    &lt;GradientStop Offset="1"/&gt;
                &lt;/LinearGradientBrush&gt;
            &lt;/Line.Stroke&gt;
        &lt;/Line&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;"> 2.Rectangle</span>

Demo:

<img class="aligncenter size-full wp-image-1794" title="Rectangle" src="http://lazynight.me/wp-content/uploads/2012/04/Rectangle.gif" alt="" width="596" height="387" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Rectangle" Width="600" Height="400"&gt;
    &lt;Grid Margin="10"&gt;
        &lt;Grid.RowDefinitions&gt;
            &lt;RowDefinition Height="160"/&gt;
            &lt;RowDefinition Height="10"/&gt;
            &lt;RowDefinition Height="160"/&gt;
        &lt;/Grid.RowDefinitions&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="180"/&gt;
            &lt;ColumnDefinition Width="10"/&gt;
            &lt;ColumnDefinition Width="180"/&gt;
            &lt;ColumnDefinition Width="10"/&gt;
            &lt;ColumnDefinition Width="180"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;!--实心填充--&gt;
        &lt;Rectangle Grid.Column="0" Grid.Row="0" Stroke="Black" Fill="LightBlue"/&gt;
        &lt;!--线性渐变--&gt;
        &lt;Rectangle Grid.Column="2" Grid.Row="0"&gt;
            &lt;Rectangle.Fill&gt;
                &lt;LinearGradientBrush StartPoint="0,0" EndPoint="1,1"&gt;
                    &lt;GradientStop Color="Pink" Offset="0"/&gt;
                    &lt;GradientStop Color="red" Offset="0.25"/&gt;
                    &lt;GradientStop Color="Orange" Offset="0.5"/&gt;
                    &lt;GradientStop Color="Green" Offset="0.75"/&gt;
                    &lt;GradientStop Color="Yellow" Offset="1"/&gt;
                &lt;/LinearGradientBrush&gt;
            &lt;/Rectangle.Fill&gt;
        &lt;/Rectangle&gt;
        &lt;!--径向渐变--&gt;
        &lt;Rectangle Grid.Column="4" Grid.Row="0"&gt;
            &lt;Rectangle.Fill&gt;
                &lt;RadialGradientBrush&gt;
                    &lt;GradientStop Color="Pink" Offset="0"/&gt;
                    &lt;GradientStop Color="Yellow" Offset="0.25"/&gt;
                    &lt;GradientStop Color="Red" Offset="0.5"/&gt;
                    &lt;GradientStop Color="Brown" Offset="0.75"/&gt;
                    &lt;GradientStop Color="Gray" Offset="1"/&gt;
                &lt;/RadialGradientBrush&gt;
            &lt;/Rectangle.Fill&gt;
        &lt;/Rectangle&gt;
        &lt;!--图片填充--&gt;
        &lt;Rectangle Grid.Column="0" Grid.Row="2"&gt;
            &lt;Rectangle.Fill&gt;
                &lt;ImageBrush ImageSource="Resources/img/aero.gif"/&gt;
            &lt;/Rectangle.Fill&gt;
        &lt;/Rectangle&gt;
        &lt;!--矢量图填充--&gt;
        &lt;Rectangle Grid.Column="2" Grid.Row="2"&gt;
            &lt;Rectangle.Fill&gt;
                &lt;DrawingBrush Viewport="0,0,0.2,0.2" TileMode="Tile"&gt;
                    &lt;DrawingBrush.Drawing&gt;
                        &lt;GeometryDrawing Brush="LightGray"&gt;
                            &lt;GeometryDrawing.Geometry&gt;
                                &lt;EllipseGeometry RadiusX="10" RadiusY="10"/&gt;
                            &lt;/GeometryDrawing.Geometry&gt;
                        &lt;/GeometryDrawing&gt;
                    &lt;/DrawingBrush.Drawing&gt;
                &lt;/DrawingBrush&gt;
            &lt;/Rectangle.Fill&gt;
        &lt;/Rectangle&gt;
        &lt;!--无填充，用线性渐变填充边线--&gt;
        &lt;Rectangle Grid.Column="4" Grid.Row="2" StrokeThickness="10"&gt;
            &lt;Rectangle.Stroke&gt;
                &lt;LinearGradientBrush StartPoint="0,0" EndPoint="1,1"&gt;
                    &lt;GradientStop Color="White" Offset="0.3"/&gt;
                    &lt;GradientStop Color="#ff4040" Offset="1"/&gt;
                &lt;/LinearGradientBrush&gt;
            &lt;/Rectangle.Stroke&gt;
        &lt;/Rectangle&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">3.Circle</span>

Demo:

<img class="aligncenter size-full wp-image-1797" title="Circle" src="http://lazynight.me/wp-content/uploads/2012/04/Circle.gif" alt="" width="200" height="200" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Circle" Width="200" Height="200"&gt;
    &lt;Grid&gt;
        &lt;Ellipse Stroke="Gray" Width="140" Height="140" Cursor="Hand" ToolTip="A Ball"&gt;
            &lt;Ellipse.Fill&gt;
                &lt;RadialGradientBrush GradientOrigin="0.2,0.8" RadiusX="0.75" RadiusY="0.75"&gt;
                    &lt;RadialGradientBrush.RelativeTransform&gt;
                        &lt;TransformGroup&gt;
                            &lt;RotateTransform Angle="90" CenterX="0.5" CenterY="0.5"/&gt;
                            &lt;TranslateTransform/&gt;
                        &lt;/TransformGroup&gt;
                    &lt;/RadialGradientBrush.RelativeTransform&gt;
                    &lt;GradientStop Color="#fff" Offset="0"/&gt;
                    &lt;GradientStop Color="Black" Offset="0.6"/&gt;
                    &lt;GradientStop Color="#ccc" Offset="1"/&gt;
                &lt;/RadialGradientBrush&gt;
            &lt;/Ellipse.Fill&gt;
        &lt;/Ellipse&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">4.Path</span>

<img class="aligncenter size-full wp-image-1798" title="Path" src="http://lazynight.me/wp-content/uploads/2012/04/Path.gif" alt="" width="334" height="350" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Path" Width="350" Height="340"&gt;
    &lt;Grid&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="160"/&gt;
            &lt;ColumnDefinition Width="160"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;Grid.RowDefinitions&gt;
            &lt;RowDefinition Height="160"/&gt;
            &lt;RowDefinition Height="160"/&gt;
        &lt;/Grid.RowDefinitions&gt;
        &lt;!--直线--&gt;
        &lt;Path Stroke="#ff4040" StrokeThickness="2" Grid.Column="0" Grid.Row="0"&gt;
            &lt;Path.Data&gt;
                &lt;LineGeometry StartPoint="20,20" EndPoint="140,140"/&gt;
            &lt;/Path.Data&gt;
        &lt;/Path&gt;
        &lt;!--矩形路径--&gt;
        &lt;Path Stroke="#ff4040" Fill="#000" Grid.Column="1" Grid.Row="0" StrokeThickness="4"&gt;
            &lt;Path.Data&gt;
                &lt;RectangleGeometry Rect="20,20,120,120" RadiusX="10" RadiusY="10"/&gt;
            &lt;/Path.Data&gt;
        &lt;/Path&gt;
        &lt;!--椭圆路径--&gt;
        &lt;Path Stroke="#FF4040" Fill="#000" Grid.Column="0" Grid.Row="1" StrokeThickness="4"&gt;
            &lt;Path.Data&gt;
                &lt;EllipseGeometry Center="80,80" RadiusX="60" RadiusY="40"/&gt;
            &lt;/Path.Data&gt;
        &lt;/Path&gt;
        &lt;!--自定义路径--&gt;
        &lt;Path Stroke="#ff4040" Fill="#000" Grid.Column="1" Grid.Row="1" StrokeThickness="4"&gt;
            &lt;Path.Data&gt;
                &lt;PathGeometry&gt;
                    &lt;PathGeometry.Figures&gt;
                        &lt;PathFigure StartPoint="25,140" IsClosed="True"&gt;
                            &lt;PathFigure.Segments&gt;
                                &lt;LineSegment Point="20,40"/&gt;
                                &lt;LineSegment Point="40,110"/&gt;
                                &lt;LineSegment Point="50,20"/&gt;
                                &lt;LineSegment Point="80,110"/&gt;
                                &lt;LineSegment Point="110,20"/&gt;
                                &lt;LineSegment Point="120,110"/&gt;
                                &lt;LineSegment Point="140,40"/&gt;
                                &lt;LineSegment Point="135,140"/&gt;
                            &lt;/PathFigure.Segments&gt;
                        &lt;/PathFigure&gt;
                    &lt;/PathGeometry.Figures&gt;
                &lt;/PathGeometry&gt;
            &lt;/Path.Data&gt;
        &lt;/Path&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<img class="aligncenter size-full wp-image-1800" title="BezierSegment" src="http://lazynight.me/wp-content/uploads/2012/04/BezierSegment.gif" alt="" width="325" height="249" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="BezierSegment" Width="350" Height="340"&gt;
    &lt;Grid&gt;
        &lt;Path Stroke="#ff4040" Fill="#000" StrokeThickness="3"&gt;
            &lt;Path.Data&gt;
                &lt;GeometryGroup&gt;
                    &lt;PathGeometry&gt;
                        &lt;PathFigure StartPoint="0,0"&gt;
                            &lt;BezierSegment Point1="250,0" Point2="50,200" Point3="300,200"/&gt;
                        &lt;/PathFigure&gt;
                    &lt;/PathGeometry&gt;
                &lt;/GeometryGroup&gt;
            &lt;/Path.Data&gt;
        &lt;/Path&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;"> 5.BitmapEffect</span>

Demo:

<img class="aligncenter size-full wp-image-1804" title="BitmapEffect" src="http://lazynight.me/wp-content/uploads/2012/04/BitmapEffect.gif" alt="" width="253" height="148" />

&nbsp;

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="BitmapEffect" Width="350" Height="340"&gt;
    &lt;Grid&gt;
        &lt;Button Content="Click Me!" Margin="20"&gt;
            &lt;Button.BitmapEffect&gt;
                &lt;DropShadowBitmapEffect Direction="-45" Opacity="0.75" ShadowDepth="10"/&gt;
            &lt;/Button.BitmapEffect&gt;
        &lt;/Button&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">6.Render Transform</span>

Demo:

<img title="Render Tranform" src="http://lazynight.me/wp-content/uploads/2012/04/Render-Tranform.gif" alt="" width="240" height="208" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Render Transform" Width="350" Height="340"&gt;
    &lt;Grid&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="Auto"/&gt;
            &lt;ColumnDefinition Width="*"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;Grid.RowDefinitions&gt;
            &lt;RowDefinition Height="Auto"/&gt;
            &lt;RowDefinition Height="*"/&gt;
        &lt;/Grid.RowDefinitions&gt;
        &lt;Button Width="80" Height="80" BorderThickness="10" BorderBrush="#ff4040" HorizontalAlignment="Left" VerticalAlignment="Top" Content="Hello."&gt;
            &lt;Button.RenderTransform&gt;
                &lt;TransformGroup&gt;
                    &lt;RotateTransform CenterX="50" CenterY="60" Angle="45"/&gt;
                    &lt;TranslateTransform X="60" Y="60"/&gt;
                    &lt;!--按钮位置偏移量--&gt;
                &lt;/TransformGroup&gt;
            &lt;/Button.RenderTransform&gt;
        &lt;/Button&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

<span style="color: #ff4040;">7.Layout Transform</span>

Demo:

<img class="aligncenter size-full wp-image-1812" title="Layout Transform" src="http://lazynight.me/wp-content/uploads/2012/04/Layout-Transform.gif" alt="" width="353" height="219" />

<pre class="brush:xml">&lt;Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="Lazy_Setter_Trigger.MainWindow"
        Title="Layout Transform" Width="350" Height="340"&gt;
    &lt;Grid&gt;
        &lt;!--Layout--&gt;
        &lt;Grid.ColumnDefinitions&gt;
            &lt;ColumnDefinition Width="Auto"/&gt;
            &lt;ColumnDefinition Width="*"/&gt;
        &lt;/Grid.ColumnDefinitions&gt;
        &lt;!--Content--&gt;
        &lt;Grid x:Name="titleBar" Background="#000"&gt;
            &lt;TextBlock Text="Lazynight" FontSize="25" Padding="5" Foreground="#ff4040" HorizontalAlignment="Left" VerticalAlignment="Bottom"&gt;
                &lt;TextBlock.LayoutTransform&gt;
                    &lt;RotateTransform Angle="-90"/&gt;
                &lt;/TextBlock.LayoutTransform&gt;
            &lt;/TextBlock&gt;
        &lt;/Grid&gt;
    &lt;/Grid&gt;
&lt;/Window&gt;</pre>

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [WPF Study notes(3):Painting][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1789.html