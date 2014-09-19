---
title: SharpPcap学习笔记3
author: Flowerowl
layout: post
permalink: /2174.html
duoshuo_thread_id:
  - 2149742
views:
  - 864
bot_views:
  - 6
categories:
  - 代码
tags:
  - SharpPcap
---
这节主要实现将捕获的数据头写入文件。

[<img class="alignnone size-full wp-image-2176" title="header" src="http://lazynight.me/wp-content/uploads/2012/05/header.gif" alt="" width="550" height="163" />][1]

[<img class="alignnone size-full wp-image-2175" title="sharppcap" src="http://lazynight.me/wp-content/uploads/2012/05/sharppcap2.gif" alt="" width="642" height="296" />][2]

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
            Console.WriteLine("Please enter the output file name:");
            //保存文件名
            string capFile = Console.ReadLine();
            //开启监听
            device.Open();
            //打开或者创建一个文件
            device.DumpOpen(capFile);
            Console.WriteLine("Listening on {0},hit 'Ctrl+C' to exit...",device.Description);
            //开始截获数据包
            device.Capture();
            //将数据流写入文件
            device.DumpFlush();
            //关闭文件
            device.DumpClose();
            //关闭监听
            device.Close();
        }
        private static void DeviceOnPacketArrival(object sender, CaptureEventArgs e)
        {
            LivePcapDevice devices = (LivePcapDevice)sender;
            if (devices.DumpOpened)
            {
                devices.Dump(e.Packet);
                Console.WriteLine("Packet dumped to file.");
            }
        }
    }
}</pre>

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [SharpPcap学习笔记3][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/05/header.gif
 [2]: http://lazynight.me/wp-content/uploads/2012/05/sharppcap2.gif
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2174.html