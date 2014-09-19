---
title: SharpPcap学习笔记1
author: Flowerowl
layout: post
permalink: /2159.html
duoshuo_thread_id:
  - 2145475
views:
  - 1440
bot_views:
  - 3
categories:
  - 代码
tags:
  - SharpPcap
---
由于想做的一个程序需要抓包分析,于是最后有了这篇文章.

写写网络通信的文章,开始咯!

> SharpPcap 是一个.NET 环境下的网络包捕获框架，基于著名的 pcap/WinPcap 库开发。提供了捕获、注入、分析和构建的功能，适用于 C# 和 VB.NET 开发语言。

[<img class="alignnone size-full wp-image-2162" title="sharppcap" src="http://lazynight.me/wp-content/uploads/2012/05/sharppcap.gif" alt="" width="671" height="435" />][1]

&nbsp;

我使用的是SharpPcap 3.1版本，如果你使用最新的4.1版本可能找不到示例代码，反正我找了半天没找到。

还有，如果用4.1版本的dll文件和之前的版本有冲突，比如说3.1示例代码中的一个类名在4.1中消失了或者被替换了，所以还需要慎重考虑哦。

以下代码主要功能是寻找本机的网络设备以及打开操纵它们并以混杂模式获取流经的数据。

<span style="color: #ff0000;">友情提示：</span>需要首先安装Winpcap文件，然后在项目里引用SharpPcap.dll和PacketDotNet.dll。

如果不安装Winpcap，编译的时候会提示你缺少wpcap。 ^_^

<pre class="lang:default decode:true">using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using SharpPcap;
using PacketDotNet;
namespace SharpPcap_Demo
{
    class Program
    {
        static void Main(string[] args)
        {
            //显示当前SharpPcap版本号
            string ver = SharpPcap.Version.VersionString;
            Console.WriteLine("SharpPcap Version : {0}",ver);

            //获取网络设备
            var devices = LivePcapDeviceList.Instance;
            if (devices.Count &lt; 1)
            {
                Console.WriteLine("No devices were found on this machine");
                return;
            }

            Console.WriteLine("The following devices are available on this machine:");
            Console.WriteLine("----------------------------------------------------");
            int i = 0;
            foreach (LivePcapDevice dev in devices)
            {
                Console.WriteLine("{0} {1} {2}",i,dev.Name,dev.Description);
                i++;
            }
            //选择一个要监听的设备
            Console.Write("Please choose a device to capture:");
            i = int.Parse(Console.ReadLine());
            LivePcapDevice device = devices[i];
            device.OnPacketArrival += new PacketArrivalEventHandler(DeviceOnPacketArrival);
            int readTimeoutMillisecond = 1000;
            //开启混杂模式
            device.Open(DeviceMode.Promiscuous,readTimeoutMillisecond);
            Console.WriteLine("Listening on {0} hit 'Enter' to stop...",device.Description);
            //开始监听进程
            device.StartCapture();
            //等待用户停止
            Console.ReadLine();
            //停止监听
            device.StopCapture();
            Console.WriteLine("Capture stopped");
            //输出统计
            Console.WriteLine(device.Statistics().ToString());
            //关闭设备
            device.Close();
        }
        private static void DeviceOnPacketArrival(object sender, CaptureEventArgs e)
        {
            var time = e.Packet.Timeval.Date;
            var len = e.Packet.Data.Length;
            Console.WriteLine("Time: {0}:{1}:{2},{3} Length: {4}",time.Hour,time.Minute,time.Second
                ,time.Millisecond,len);
            Console.WriteLine(e.Packet.ToString());
        }
    }
}</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [SharpPcap学习笔记1][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/05/sharppcap.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2159.html