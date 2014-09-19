---
title: Node.js连接MYSQL
author: Flowerowl
layout: post
permalink: /1963.html
duoshuo_thread_id:
  - 1267020
views:
  - 3905
bot_views:
  - 1
categories:
  - Node.js
  - 代码
---
  
弄了一上午，犯了很幼稚的错误，浪费了4个小时，不过也不算浪费时间，学习新知识就这样，得自己慢慢摸索，摸索的过程不算浪费时间&#8230;

好吧，今天介绍Node.js连接Mysql。

CMD下运行npm install mysql，默认安装到node/node_modules下，如图:

<img class="aligncenter size-full wp-image-1964" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/node-mysql.gif" alt="" width="747" height="550" />

<span style="color: #ff4040;">注意：这只是安装了Node.js需要的Mysql模块，具体运行还需要本地安装Mysql！上午就被卡在这里4个小时，希望你能够看到这个提示&#8230;避免再浪费时间。</span>

如果你能直接想到要安装Mysql，那就是我智商有点低下了&#8230;

好吧，现在开启你的本地Mysql服务，我直接用的WAMP搞定的，查看进程里是否有一个mysqlid.exe在运行。

然后去建表：

<img class="aligncenter size-full wp-image-1967" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/post.gif" alt="" width="457" height="360" />

最后，开始写代码！

<pre class="brush:js">var sys=require('util');
console.log('Connecting to MYSQL...');
var client=require('mysql').createClient({
	'host':'localhost',
	'port':'3306',
	'user':'root',
	'password':''
});
var date=new Date();
var createtime=date.getFullYear() + '-' + (date.getMonth() + 1) + '-' + date.getDate();
console.log(createtime);
console.log('Connected to MYSQL automatically.');

ClientConnectionReady=function(client){
	console.log('ClientConnectionReady.');
	client.query('USE LazyBlog',function(error,results){
		if(error){
			console.log('ClientConnectionReady Error:'+error.message);
			client.end();
			return;
		}
		ClientReady(client);
	});
}

ClientReady=function(client){
	var values=['Hello!','Lazynight',createtime];
	client.query('INSERT INTO posts SET title=?,content=?,createtime=?',values,
	function(error,results){
		if(error){
			console.log('ClientReady Error:'+error.message);
			client.end();
			return;
		}
		console.log('Inserted:'+results.affectedRows+'row.');
		console.log('Id inserted:'+results.insertId);
	}
	);
	GetData(client);
}

GetData=function(client){
	client.query(
		'SELECT * FROM posts',
		function selectZ(error,results,fields){
			if(error){
				console.log('GetData Error:'+error.message);
				client.end();
				return;
			}
			console.log('Results:');
			console.log(results);
			console.log('Field metadata:');
			console.log('fields');
			console.log(sys.inspect(results));
			if(results.length&gt;0){
				var firstResult=results[0];
				console.log('Title:'+firstResult['title']);
				console.log('Content:'+firstResult['content']);
				console.log('Createtime:'+firstResult['createtime']);

			}
		}
	);
	client.end();
	console.log('Connection closed.');
}

ClientConnectionReady(client);</pre>

运行结果：

<img class="aligncenter size-full wp-image-1965" title="Lazynight" src="http://lazynight.me/wp-content/uploads/2012/04/node.gif" alt="" width="847" height="549" />

&nbsp;

OK，自己折腾吧，祝玩得愉快。

转载请注明：[于哲的博客][1] &raquo; [Node.js连接MYSQL][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1963.html