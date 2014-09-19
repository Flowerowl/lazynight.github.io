---
title: 修改WordPress默认评论样式
author: Flowerowl
layout: post
permalink: /958.html
duoshuo_thread_id:
  - 1266923
views:
  - 2569
bot_views:
  - 2
categories:
  - Wordpress
  - 代码
---
  
我们在使用Wordpress的时候，默认的侧栏评论样式是&#8221;评论者ON文章&#8221;，这样很不直观，惹人心烦。想直接显示评论内容吗？方法如下：

找到\www\wordpress\wp-includes文件夹下的default-widgets.php，用Norepad++等编辑器编辑。(勿用windows记事本)  
找到如下内容：

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #c0c0c0;">if ( $comments ) {</span><br /> <span style="color: #c0c0c0;">foreach ( (array) $comments as $comment) {</span><br /> <span style="color: #c0c0c0;">$output .= ‘<li class=”recentcomments”>’ . /* translators: comments widget: 1: comment author, 2: post link */ sprintf(_x(‘%1$s on %2$s’, ‘widgets’), get_comment_author_link(), ‘<a href=”‘ . esc_url( get_comment_link($comment->comment_ID) ) . ‘”>’ . get_the_title($comment->comment_post_ID) . ‘</a>’) . ‘</li>’;</span><br /> <span style="color: #c0c0c0;">}</span><br /> <span style="color: #c0c0c0;">}</span>
</div>

我把评论者去掉了，只剩下评论内容：

<div class="source" style="font-family: '[object HTMLOptionElement]', Consolas, 'Lucida Console', 'Courier New'; color: #c0c0c0; background-color: #000000;">
  <span style="color: #c0c0c0;">if ( $comments ) {</span><br /> <span style="color: #c0c0c0;">foreach ( (array) $comments as $comment) {</span><br /> <span style="color: #c0c0c0;">$output .=  &#8216;<li class=&#8221;recentcomments&#8221;>&#8217; . /* translators: comments widget: 1: comment author, 2: post link */ sprintf(_x(&#8216;%1$s&#8217;, &#8216;widgets&#8217;),  &#8216;<a href=&#8221;&#8216; . esc_url( get_comment_link($comment->comment_ID) ) . &#8216;&#8221;>&#8217; .  strip_tags( $comment->comment_content) . &#8216;</a>&#8217;) . &#8216;</li>&#8217;;</span><br /> <span style="color: #c0c0c0;">}</span><br /> <span style="color: #c0c0c0;">}</span>
</div>

修改之后效果：

<img class="aligncenter size-full wp-image-960" title="Lazynight | 夜阑" src="http://lazynight.me/wp-content/uploads/2011/11/20111126182629.jpg" alt="" width="755" height="141" />

转载请注明：[于哲的博客][1] &raquo; [修改WordPress默认评论样式][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/958.html