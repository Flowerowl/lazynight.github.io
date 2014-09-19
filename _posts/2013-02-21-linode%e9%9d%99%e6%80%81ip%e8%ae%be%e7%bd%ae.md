---
title: Linode静态IP设置
author: Flowerowl
layout: post
permalink: /2803.html
views:
  - 1508
duoshuo_thread_id:
  - 1220743779864322453
bot_views:
  - 2
categories:
  - 实用
  - 技术杂谈
tags:
  - IP
  - Linode
  - VPS
  - 静态
---
每个linode VPS都会配备至少一个静态ip地址，这个地址就在你的“系统控制面板” 的“Remote Access”选项卡里，“Public IPs”所列出来的ip地址，就是你所拥有的静态ip地址。因为linode vps的静态ip地址是以DHCP模式有上一级系统自动分配的，所以为安全起见，在你的系统安装完毕、第一次启动以后，你需要做的就是将这（些）个ip地 址依次填入系统的相关控制文件内，达到真正“静态”的目的。做法如下：

1. 对于Debian / Ubuntu系统：需要修改这个文件 /etc/network/interfaces，按以下格式依次填写

\# The loopback interface  
auto lo  
iface lo inet loopback

\# Configuration for eth0 and aliases

\# This line ensures that the interface will be brought up during boot.  
auto eth0 eth0:0 eth0:1 #系统启动时需要随机启动的网卡（别名）

\# eth0 &#8211; This is the main IP address that will be used for most outbound connections.  
\# The address, netmask and gateway are all necessary.  
iface eth0 inet static #eth0配置信息  
address 12.34.56.78 #ip地址  
netmask 255.255.255.0 #子网掩码  
gateway 12.34.56.1 #ip地址的网关

\# eth0:0  
\# This is a second public IP address.  
iface eth0:0 inet static #网卡eth0:0的配置信息  
address 34.56.78.90 #ip地址  
netmask 255.255.255.0 #子网掩码

\# eth0:1 &#8211; Private IPs have no gateway (they are not publicly routable) so all you need to  
\# specify is the address and netmask.  
iface eth0:1 inet static  
address 192.168.133.234  
netmask 255.255.128.0

Debian/Ubuntu的网卡命名规则比较容易理解，其中：eth0代表你的网卡，此端口匹配第一个ip地址；eth0:0代表你的第二个网 卡，但因为只有一个物理网卡，为和之前的区分，则以“interface:alias_name”的规则加以区分命名，此端口匹配第二个ip地址；以此类 推，当你拥有多个ip地址时，分别以eth0/eth0:0/eth0:1/eth0:2等来匹配。最后一个网卡是用来匹配你的linode vps的内网ip的，也就是192.168开头的ip。

在配置完毕之后，在ssh界面输入以下信息，重启网卡，让以上设置生效：/etc/init.d/networking restart，之后为了验证以上设置是否正确、是否生效，你可以ping一下你的默认网关ip，这个网关ip是在“Remote Access”下的“Default Gateways”中的，如果能ping通，则代表成功！反之请检查以上的设置！

因为你的linode vps只有一张物理网卡的缘故，只需配置一个网关即可，一般在第一个ip地址中配置，剩下的ip地址无须配置。

2. 对于Centos系统，要修改的文件不止一个，你拥有的ip地址越多，需要修改的文件越多；该系统与Debian/Ubuntu的区别在于，D/U系统在 一个文件中对所有设置做了定义，而C系统则要每个网卡对应一个配置文件，并且文件的命名方式为 ifcfg-<interface\_alias\_name>，也就是ifcfg-eth0/ifcfg-eth0:0/ifcfg- eth0:1/ifcfg-eth0:2，以此类推，而这些文件统一放置在/etc/sysconfig/network-scripts/文件夹下。每个配置文件的具体设置如下：

第一个网卡配置信息:/etc/sysconfig/network-scripts/ifcfg-eth0

\# Configuration for eth0  
DEVICE=eth0 #网卡eth0  
BOOTPROTO=none #网卡ip的绑定方式,这里选择none

\# This line ensures that the interface will be brought up during boot.  
ONBOOT=yes #该网卡是否随机启动

\# eth0 &#8211; This is the main IP address that will be used for most outbound connections.  
\# The address, netmask and gateway are all necessary.  
IPADDR=12.34.56.78 #ip地址  
NETMASK=255.255.255.0 #子网掩码  
GATEWAY=12.34.56.1 #ip地址网关

第二个网卡配置信息:/etc/sysconfig/network-scripts/ifcfg-eth0:0

\# Configuration for eth0:0  
DEVICE=eth0:0  
BOOTPROTO=none

\# This line ensures that the interface will be brought up during boot.  
ONBOOT=yes

\# eth0:0  
IPADDR=34.56.78.90  
NETMASK=255.255.255.0

第三个网卡配置信息:/etc/sysconfig/network-scripts/ifcfg-eth0:1

\# Configuration for eth0:1  
DEVICE=eth0:1  
BOOTPROTO=none

\# This line ensures that the interface will be brought up during boot.  
ONBOOT=yes

\# eth0:1 &#8211; Private IPs have no gateway (they are not publicly routable) so all you need to  
\# specify is the address and netmask.  
IPADDR=192.168.133.234  
NETMASK=255.255.128.0

配置完毕后，剩下的工作也是重启网卡，在ssh中输入：service network restart，未验证是否成功，ping一下你的网关ip即可。因为你的linode vps只有一张物理网卡的缘故，只需配置一个网关即可，一般在第一个ip地址中配置，剩下的ip地址无须配置。

<div id="xunlei_com_thunder_helper_plugin_d462f475-c18e-46be-bd10-327458d045bd">
</div>

转载请注明：[于哲的博客][1] &raquo; [Linode静态IP设置][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2803.html