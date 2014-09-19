---
title: WordPress 添加投稿页面
author: Flowerowl
layout: post
permalink: /2706.html
views:
  - 1893
duoshuo_thread_id:
  - 1220743779864322416
bot_views:
  - 2
categories:
  - php
  - Wordpress
  - 代码
tags:
  - Wordpress
  - 投稿
---
其实投稿功能很早就有了，最近只是突然想起来要整理一下小站，增加一个投稿界面吧，这样也不错。

以下代码参考露兜博客（这里我添加了一个kindeditor网页文本编辑器，你可以使用simple版本）：

这样是创建一个tougao模板，新建一个“投稿”页面，选择此模板即可。

另外还可以使用插件来实现。

[<img class="alignnone size-full wp-image-2707" title="tougao" src="http://lazynight.me/wp-content/uploads/2012/11/tougao.jpg" alt="" width="826" height="499" />][1]

<pre class="lang:php decode:true" title="投稿功能简单实现">&lt;?php
/**
 * Template Name: tougao
 * 作者：露兜
 * 博客：http://www.ludou.org/
 * 
 * 更新记录
 *  2010年09月09日 ：
 *  首个版本发布
 *  
 *  2011年03月17日 ：
 *  修正时间戳函数，使用wp函数current_time('timestamp')替代time()
 *  
 *  2011年04月12日 ：
 *  修改了wp_die函数调用，使用合适的页面title
 */

if( isset($_POST['tougao_form']) && $_POST['tougao_form'] == 'send')
{
    global $wpdb;
    $last_post = $wpdb-&gt;get_var("SELECT post_date FROM $wpdb-&gt;posts WHERE post_type = 'post' ORDER BY post_date DESC LIMIT 1");

    // 博客当前最新文章发布时间与要投稿的文章至少间隔120秒。
    // 可自行修改时间间隔，修改下面代码中的120即可
    // 相比Cookie来验证两次投稿的时间差，读数据库的方式更加安全
    if ( current_time('timestamp') - strtotime($last_post) &lt; 120 )
    {
        wp_die('您投稿也太勤快了吧，先歇会儿！');
    }

    // 表单变量初始化
    $name = isset( $_POST['tougao_authorname'] ) ? trim(htmlspecialchars($_POST['tougao_authorname'], ENT_QUOTES)) : '';
    $email =  isset( $_POST['tougao_authoremail'] ) ? trim(htmlspecialchars($_POST['tougao_authoremail'], ENT_QUOTES)) : '';
    $blog =  isset( $_POST['tougao_authorblog'] ) ? trim(htmlspecialchars($_POST['tougao_authorblog'], ENT_QUOTES)) : '';
    $title =  isset( $_POST['tougao_title'] ) ? trim(htmlspecialchars($_POST['tougao_title'], ENT_QUOTES)) : '';
    $category =  isset( $_POST['cat'] ) ? (int)$_POST['cat'] : 0;
    $content =  isset( $_POST['tougao_content'] ) ? trim(htmlspecialchars($_POST['tougao_content'], ENT_QUOTES)) : '';

    // 表单项数据验证
    if ( empty($name) || mb_strlen($name) &gt; 20 )
    {
        wp_die('昵称必须填写，且长度不得超过20字');
    }

    if ( empty($email) || strlen($email) &gt; 60 || !preg_match("/^([a-z0-9\+_\-]+)(\.[a-z0-9\+_\-]+)*@([a-z0-9\-]+\.)+[a-z]{2,6}$/ix", $email))
    {
        wp_die('Email必须填写，且长度不得超过60字，必须符合Email格式');
    }

    if ( empty($title) || mb_strlen($title) &gt; 100 )
    {
        wp_die('标题必须填写，且长度不得超过100字');
    }

    if ( empty($content) || mb_strlen($content) &gt; 3000 || mb_strlen($content) &lt; 100)
    {
        wp_die('内容必须填写，且长度不得超过3000字，不得少于100字');
    }

    $post_content = '昵称: '.$name.'&lt;br /&gt;Email: '.$email.'&lt;br /&gt;blog: '.$blog.'&lt;br /&gt;内容:&lt;br /&gt;'.$content;

    $tougao = array(
        'post_title' =&gt; $title,
        'post_content' =&gt; $post_content,
        'post_category' =&gt; array($category)
    );

    // 将文章插入数据库
    $status = wp_insert_post( $tougao );

    if ($status != 0) 
    { 
        // 投稿成功给博主发送邮件
        // somebody#example.com替换博主邮箱
        // My subject替换为邮件标题，content替换为邮件内容
        wp_mail("somebody#example.com","My subject","content");

        wp_die('投稿成功！感谢投稿！', '投稿成功');
    }
    else
    {
        wp_die('投稿失败！');
    }
}
get_header(); ?&gt;
&lt;link rel="stylesheet" href="http://localhost/wordpress/kindeditor/themes/simple/simple.css" /&gt;
&lt;script charset="utf-8" src="http://localhost/wordpress/kindeditor/kindeditor.js"&gt;&lt;/script&gt;
&lt;script charset="utf-8" src="http://localhost/wordpress/kindeditor/lang/zh_CN.js"&gt;&lt;/script&gt;
&lt;script&gt;
        var editor;
        KindEditor.ready(function(K) {
                editor = K.create('#tougao_content', {
                        themeType : 'simple'
                });
        });
&lt;/script&gt;
&lt;div class="content-wrap"&gt;
	&lt;div class="content"&gt;
	&lt;?php while (have_posts()) : the_post(); ?&gt;
		&lt;h1 class="meta-tit"&gt;&lt;?php the_title(); ?&gt;&lt;/h1&gt;
		&lt;p class="meta-info"&gt;&lt;?php the_date_xml(); ?&gt; &nbsp;&nbsp; &lt;?php comments_popup_link('抢沙发', '1条评论', '%条评论'); ?&gt; &nbsp;&nbsp; &lt;?php if(function_exists('the_views')) the_views(); ?&gt;人浏览 &nbsp;&nbsp; &lt;?php edit_post_link('[编辑]'); ?&gt;&lt;/p&gt;

		&lt;div class="entry"&gt;
			&lt;?php the_content(); ?&gt;

			&lt;!-- 关于表单样式，请自行调整--&gt;
			&lt;form method="post" action="&lt;?php echo $_SERVER["REQUEST_URI"]; ?&gt;"&gt;
			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label for="tougao_authorname"&gt;昵称:*&lt;/label&gt;
			        &lt;input type="text" size="40" value="" id="tougao_authorname" name="tougao_authorname" /&gt;
			    &lt;/div&gt;

			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label for="tougao_authoremail"&gt;E-Mail:*&lt;/label&gt;
			        &lt;input type="text" size="40" value="" id="tougao_authoremail" name="tougao_authoremail" /&gt;
			    &lt;/div&gt;

			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label for="tougao_authorblog"&gt;您的博客:&lt;/label&gt;
			        &lt;input type="text" size="40" value="" id="tougao_authorblog" name="tougao_authorblog" /&gt;
			    &lt;/div&gt;

			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label for="tougao_title"&gt;文章标题:*&lt;/label&gt;
			        &lt;input type="text" size="40" value="" id="tougao_title" name="tougao_title" /&gt;
			    &lt;/div&gt;

			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label for="tougaocategorg"&gt;分类:*&lt;/label&gt;
			        &lt;?php wp_dropdown_categories('id=tougaocategorg&show_count=1&hierarchical=1'); ?&gt;
			    &lt;/div&gt;

			    &lt;div style="text-align: left; padding-top: 10px;"&gt;
			        &lt;label style="vertical-align:top" for="tougao_content"&gt;文章内容:*&lt;/label&gt;
			        &lt;textarea rows="15" cols="55" id="tougao_content" name="tougao_content"&gt;&lt;/textarea&gt;
			    &lt;/div&gt;

			    &lt;br clear="all"&gt;
			    &lt;div style="text-align: center; padding-top: 10px;"&gt;
			        &lt;input type="hidden" value="send" name="tougao_form" /&gt;
			        &lt;input type="submit" value="提交" /&gt;
			        &lt;input type="reset" value="重填" /&gt;
			    &lt;/div&gt;
			&lt;/form&gt;
		&lt;/div&gt;

	&lt;?php endwhile;  ?&gt;
	&lt;/div&gt;
&lt;/div&gt;

&lt;?php get_sidebar(); get_footer(); ?&gt;</pre>

&nbsp;

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [WordPress 添加投稿页面][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/11/tougao.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2706.html