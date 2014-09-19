---
title: SharpPcap学习笔记2
author: Flowerowl
layout: post
permalink: /2168.html
duoshuo_thread_id:
  - 2148715
views:
  - 1019
bot_views:
  - 3
categories:
  - 代码
tags:
  - SharpPcap
---
这节的代码来自官方示例第6个，主要功能是实时获取Tcp/ip数据包的源地址及端口号还有目的地址及其端口号。

[<img class="alignnone size-full wp-image-2170" title="sharppcap" src="http://lazynight.me/wp-content/uploads/2012/05/sharppcap1.gif" alt="" width="668" height="438" />][1]

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
            //过滤tcp/ip包
            string filter = "ip and tcp";
            device.SetFilter(filter);
            Console.WriteLine("The following tcpdump filter will be applied:{0}",filter);
            Console.WriteLine("Listening on {0},hit 'Ctrl+C' to exit...",device.Description);
            //开始监听进程,不停止
            device.Capture();
            //关闭设备
            device.Close();
        }
        private static void DeviceOnPacketArrival(object sender, CaptureEventArgs e)
        {
            var time = e.Packet.Timeval.Date;
            var len = e.Packet.Data.Length;
            var packet = PacketDotNet.Packet.ParsePacket(e.Packet);
            var tcpPacket = PacketDotNet.TcpPacket.GetEncapsulated(packet);
            if (tcpPacket != null)
            {
                var ipPacket = (PacketDotNet.IpPacket)tcpPacket.ParentPacket;
                //数据包源地址
                System.Net.IPAddress srcIp = ipPacket.SourceAddress;
                //数据包目的地址
                System.Net.IPAddress dstIp = ipPacket.DestinationAddress;
                //源地址端口
                int srcPort = tcpPacket.SourcePort;
                //目的地址端口
                int dstPort = tcpPacket.DestinationPort;
                Console.WriteLine("{0}:{1}:{2},{3} Len={4} {5}:{6} -&gt; {7}:{8}",time.Hour,time.Minute,time.Second,time.Millisecond,len,srcIp,srcPort,dstIp,dstPort);
            }
        }
    }
}</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [SharpPcap学习笔记2][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/05/sharppcap1.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2168.html