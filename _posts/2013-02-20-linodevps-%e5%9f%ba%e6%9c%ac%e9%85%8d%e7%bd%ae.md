---
title: LinodeVPS 基本配置
author: Flowerowl
layout: post
permalink: /2802.html
views:
  - 924
duoshuo_thread_id:
  - 1220743779864322452
bot_views:
  - 2
categories:
  - 技术杂谈
tags:
  - CentOS
  - Linode
  - Linux
  - VPS
---
花了我两天的时间鼓捣VPS，就因为GFW把东京的还有美国的一部分IP给屏蔽掉了，怎么ping都不通。

前前后后和客服发邮件，发邮件，解决此事。最后终于！ping通了，ssh连上了。

ok，既然没什么问题了，就开始配置吧，首先yum update，升级一下软件包。（系统为CentOS6.2 64bit）

完了之后修改一下hostname

<pre>echo "HOSTNAME=lazynight" >> /etc/sysconfig/network
hostname "lazynight"</pre>

**File:***/etc/hosts*

<div>
  <pre class="crayon-selected">127.0.0.1        localhost.localdomain    localhost
12.34.56.78      lazynight.example.com        lazynight
2600:3c01::a123:b456:c789:d012  lazynight.example.com        lazynight</pre>
</div>

[root@li539-40 /]# hostname  
lazynight

接下来改一下时区：

对于中国用户，如果centos默认使用UTC时区，那时间相差八个小时，其实可以通过简单的设置，变为中国时区，这时候机器上的时间和本地手表上的时间就是一致的。执行如下命令：

<pre>cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime</pre>

就可以了，如果没有看到/Asia/Shanghai文件，手动执行以下 tzselect ，按照提示选择即可。

[root@li539-40 /]# date  
2013年 02月 20日 星期三 22:12:38 CST

详细教程在这里=><span style="color: #ff0000;"><a href="http://library.linode.com/getting-started" target="_blank"><span style="color: #ff0000;">http://library.linode.com/getting-started</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [LinodeVPS 基本配置][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2802.html