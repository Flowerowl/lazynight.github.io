---
title: Linux下mysql新建账号及权限设置
author: Flowerowl
layout: post
permalink: /2492.html
duoshuo_thread_id:
  - 8707088
views:
  - 867
bot_views:
  - 2
categories:
  - Linux
  - Mysql
  - 技术杂谈
---
1、权限赋予

<div id="cnblogs_post_body">
  <p>
    说明：mysql部署在服务器A上，内网上主机B通过客户端工具连接服务器A以进行数据库操作，需要服务器A赋予主机B操作mysql的权限
  </p>
  
  <p>
    1.1 在服务器A上进入mysql,假设在服务器A上mysql的账户是root：
  </p>
  
  <div>
    <pre>mysql -u root -p</pre>
  </div>
  
  <p>
    然后回车键入密码！
  </p>
  
  <p>
    1.2 赋予主机B操作数据库的权限
  </p>
  
  <div>
    <pre>mysql> grant usage on *.* to username@192.168.0.1 identified by 'password';</pre>
  </div>
  
  <p>
    说明：赋予username@192.168.0.1 使用所有数据库的权限，在主机192.168.0.1上使用username账户登录，密码为：password
  </p>
  
  <div>
    <pre>mysql> grant usage on *.* to username identified by 'password';</pre>
  </div>
  
  <p>
    说明：赋予username使用所有数据库的权限，在所有主机上使用username账户登录，密码为：password
  </p>
  
  <div>
    <pre>mysql> grant all privileges on newdb.* to username@192.168.0.1;</pre>
  </div>
  
  <p>
    说明：赋予username@192.168.0.1 操作数据库newdb的最高权限，在主机192.168.0.1上使用username账户登录，无密码
  </p>
  
  <p>
    举例：
  </p>
  
  <div>
    <pre>mysql> grant all privileges on *.* to root@192.168.0.1 identified by '123456' ;</pre>
  </div>
  
  <p>
    说明：赋予root@192.168.0.1 使用所有数据库的权限，在主机192.168.0.1上使用root账户登录，密码为：123456
  </p>
  
  <p>
    2、移除账号
  </p>
  
  <div>
    <pre>mysql> drop user root@192.168.0.1;</pre>
  </div>
  
  <p>
    说明：移除账户root,这样，主机192.168.0.1就不再可以使用root用户操作服务器A上的数据库
  </p>
</div>

转载请注明：[于哲的博客][1] &raquo; [Linux下mysql新建账号及权限设置][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2492.html