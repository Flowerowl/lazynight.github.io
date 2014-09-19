---
title: '解决Warning: scandir() has been disabled for security reasons in…的问题'
author: Flowerowl
layout: post
permalink: /2842.html
views:
  - 1597
duoshuo_thread_id:
  - 1220743779864322467
bot_views:
  - 1
categories:
  - 技术杂谈
tags:
  - lnmp
  - scandir
  - wp
  - 主题
---
最近折腾wp，主题无法编辑，提示错误：

> Warning: scandir() has been disabled for security reasons in /home/wwwroot/yourdomain.com/wp-includes/class-wp-theme.php on line 978
> 
> Warning: Invalid argument supplied for foreach() in /home/wwwroot/yourdomain.com/wp-includes/class-wp-theme.php on line 981
> 
> Warning: scandir() has been disabled for security reasons in /home/wwwroot/yourdomain.com/wp-includes/class-wp-theme.php on line 978
> 
> Warning: Invalid argument supplied for foreach() in /home/wwwroot/yourdomain.com/wp-includes/class-wp-theme.php on line 981

原因：LNMP 0.9禁用了部分存在危险的PHP函数

> LNMP0.9禁用的PHP函数包括：passthru, exec, system, chroot, scandir, chgrp, chown, shell\_exec, proc\_open, proc\_get\_status, ini\_alter, ini\_alter, ini\_restore, dl, pfsockopen ,openlog, syslog, readlink, symlink, popepassthru, stream\_socket_server, fsocket, fsockopen

### <span style="color: #ff0000;">解决方法：</span> {#solution}

编辑PHP配置文件：

<pre>vi /usr/local/php/etc/php.ini</pre>

寻找disable\_functions字符串，将后面的scandir删除（提示：vi下可输入/，进入搜索模式，轻松找到disable\_functions）

重启PHP生效

<pre>/etc/init.d/php-fpm restart</pre>

转载请注明：[于哲的博客][1] &raquo; [解决Warning: scandir() has been disabled for security reasons in…的问题][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2842.html