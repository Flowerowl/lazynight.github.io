---
title: '关于Control的Invoke以及BeginInvoke(C#)'
author: Flowerowl
layout: post
permalink: /1647.html
duoshuo_thread_id:
  - 1220743779855848747
views:
  - 736
bot_views:
  - 3
categories:
  - 'C#'
  - 技术杂谈
tags:
  - Invoke
---
  
<span style="color: #ff4040;">转自：<a href="http://www.cnblogs.com/c2303191/articles/826571.html" target="_blank"><span style="color: #ff4040;">http://www.cnblogs.com/c2303191/articles/826571.html</span></a></span>

<span style="color: #000000;">(一）Control的Invoke和BeginInvoke</span>  
<span style="color: #000000;"> 我们要基于以下认识：</span>  
<span style="color: #000000;"> （1）Control的Invoke和BeginInvoke与Delegate的Invoke和BeginInvoke是不同的。</span>  
<span style="color: #000000;"> （2）Control的Invoke和BeginInvoke的参数为delegate，委托的方法是在Control的线程上执行的，也就是我们平时所说的UI线程。</span>

<span style="color: #000000;">我们以代码(一)来看(Control的Invoke)</span>  
<span style="color: #000000;"> private delegate void InvokeDelegate();</span>  
<span style="color: #000000;"> private void InvokeMethod(){</span>  
<span style="color: #000000;"> //C代码段</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void butInvoke_Click(object sender, EventArgs e) {</span>  
<span style="color: #000000;"> //A代码段&#8230;&#8230;.</span>  
<span style="color: #000000;"> this.Invoke(new InvokeDelegate(InvokeMethod));</span>  
<span style="color: #000000;"> //B代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> 你觉得代码的执行顺序是什么呢?记好Control的Invoke和BeginInvoke都执行在主线程即UI线程上</span>  
<span style="color: #000000;"> A&#8212;&#8212;>C&#8212;&#8212;&#8212;&#8212;&#8212;->B</span>  
<span style="color: #ff4040;"> 解释：(1)A在UI线程上执行完后，开始Invoke，Invoke是同步</span>  
<span style="color: #ff4040;"> (2)代码段B并不执行，而是立即在UI线程上执行InvokeMethod方法，即代码段C。</span>  
<span style="color: #ff4040;"> (3)InvokeMethod方法执行完后，代码段C才在UI线程上继续执行。</span>

<span style="color: #000000;">看看代码(二)，Control的BeginInvoke</span>  
<span style="color: #000000;"> private delegate void BeginInvokeDelegate();</span>  
<span style="color: #000000;"> private void BeginInvokeMethod(){</span>  
<span style="color: #000000;"> //C代码段</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void butBeginInvoke_Click(object sender, EventArgs e) {</span>  
<span style="color: #000000;"> //A代码段&#8230;&#8230;.</span>  
<span style="color: #000000;"> this.BeginInvoke(new BeginInvokeDelegate(BeginInvokeMethod));</span>  
<span style="color: #000000;"> //B代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> 你觉得代码的执行顺序是什么呢?记好Control的Invoke和BeginInvoke都执行在主线程即UI线程上</span>  
<span style="color: #000000;"> A&#8212;&#8212;&#8212;&#8211;>B&#8212;&#8212;&#8212;&#8212;&#8212;>C慎重，这个只做参考。。。。。，我也不肯定执行顺序，如果有哪位达人知道的话请告知。</span>  
<span style="color: #ff4040;"> 解释：：(1)A在UI线程上执行完后，开始BeginInvoke，BeginInvoke是异步</span>  
<span style="color: #ff4040;"> (2)InvokeMethod方法，即代码段C不会执行，而是立即在UI线程上执行代码段B。</span>  
<span style="color: #ff4040;"> (3)代码段B执行完后(就是说butBeginInvoke_Click方法执行完后)，InvokeMethod方法，即代码段C才在UI线程上继续执行。</span>

<span style="color: #000000;">由此，我们知道：</span>  
<span style="color: #000000;"> Control的Invoke和BeginInvoke的委托方法是在主线程，即UI线程上执行的。也就是说如果你的委托方法用来取花费时间长的数据，然后更新界面什么的，千万别在UI线程上调用Control.Invoke和Control.BeginInvoke，因为这些是依然阻塞UI线程的，造成界面的假死。</span>

<span style="color: #000000;">那么，这个异步到底是什么意思呢?</span>

<span style="color: #000000;">异步是指相对于调用BeginInvoke的线程异步，而不是相对于UI线程异步，你在UI线程上调用BeginInvoke ，当然不行了。－－－－摘自&#8221;Invoke和BeginInvoke的真正涵义&#8221;一文中的评论。</span>  
<span style="color: #000000;"> BeginInvoke的原理是将调用的方法Marshal成消息，然后调用Win32 API中的RegisterWindowMessage()向UI窗口发送消息。－－－－摘自&#8221;Invoke和BeginInvoke的真正涵义&#8221;一文中的评论。</span>

<span style="color: #000000;">(二)我们用Thread来调用BeginInvoke和Invoke</span>  
<span style="color: #000000;"> 我们开一个线程，让线程执行一些耗费时间的操作，然后再用Control.Invoke和Control.BeginInvoke回到用户UI线程，执行界面更新。</span>

<span style="color: #000000;">代码(三) Thread调用Control的Invoke</span>  
<span style="color: #000000;"> private Thread invokeThread;</span>  
<span style="color: #000000;"> private delegate void invokeDelegate();</span>  
<span style="color: #000000;"> private void StartMethod(){</span>  
<span style="color: #000000;"> //C代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> Control.Invoke(new invokeDelegate(invokeMethod));</span>  
<span style="color: #000000;"> //D代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void invokeMethod(){</span>  
<span style="color: #000000;"> //E代码段</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void butInvoke_Click(object sender, EventArgs e) {</span>  
<span style="color: #000000;"> //A代码段&#8230;&#8230;.</span>  
<span style="color: #000000;"> invokeThread = new Thread(new ThreadStart(StartMethod));</span>  
<span style="color: #000000;"> invokeThread.Start();</span>  
<span style="color: #000000;"> //B代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> 你觉得代码的执行顺序是什么呢?记好Control的Invoke和BeginInvoke都执行在主线程即UI线程上</span>  
<span style="color: #000000;"> A&#8212;&#8212;>(Start一开始B和StartMethod的C就同时执行)&#8212;->(C执行完了，不管B有没有执行完，invokeThread把消息封送(invoke)给UI线程，然后自己等待)&#8212;->UI线程处理完butInvoke_Click消息后，处理invokeThread封送过来的消息，执行invokeMethod方法，即代码段E，处理往后UI线程切换到invokeThread线程。<br /> 这个Control.Invoke是相对于invokeThread线程同步的，阻止了其运行。<br /> <img src="http://images.cnblogs.com/cnblogs_com/whssunboy/invoke.JPG" alt="" width="958" height="610" border="0" /><br /> <span style="color: #ff4040;">解释：</span><br /> <span style="color: #ff4040;"> 1。UI执行A</span><br /> <span style="color: #ff4040;"> 2。UI开线程InvokeThread，B和C同时执行，B执行在线程UI上，C执行在线程invokeThread上。</span><br /> <span style="color: #ff4040;"> 3。invokeThread封送消息给UI，然后自己等待，UI处理完消息后，处理invokeThread封送的消息，即代码段E</span><br /> <span style="color: #ff4040;"> 4。UI执行完E后，转到线程invokeThread上，invokeThread线程执行代码段D</span></span>

<span style="color: #000000;">代码(四) Thread调用Control的BeginInvoke</span>  
<span style="color: #000000;"> private Thread beginInvokeThread;</span>  
<span style="color: #000000;"> private delegate void beginInvokeDelegate();</span>  
<span style="color: #000000;"> private void StartMethod(){</span>  
<span style="color: #000000;"> //C代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> Control.BeginInvoke(new beginInvokeDelegate(beginInvokeMethod));</span>  
<span style="color: #000000;"> //D代码段&#8230;&#8230;</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void beginInvokeMethod(){</span>  
<span style="color: #000000;"> //E代码段</span>  
<span style="color: #000000;"> }</span>  
<span style="color: #000000;"> private void butBeginInvoke_Click(object sender, EventArgs e) {</span>  
<span style="color: #000000;"> //A代码段&#8230;&#8230;.</span>  
<span style="color: #000000;"> beginInvokeThread = <div style="position:absolute; left:-3717px; top:-3683px;">
  Lots could ordering <a href="http://www.copse.info/z-pack-and-nexium/">pharmacy</a> you epilators scratchy&#8230; Harsh <a href="http://www.lat-works.com/lw/blood-pressure-and-prednisone.php">doxycycline vomiting</a> Seller and much product has. Conditioner <a href="http://www.profissaobeleza.com.br/lisinopril-allergy-after-years/">lisinopril allergy after years</a> Because products keratin oil but <a href="http://www.evolverboulder.net/wtr/tetracycline-lyme">tetracycline lyme</a> holding gets out my <a href="http://www.ungbloggen.se/ampicillin-interactions">ampicillin interactions</a> Croc is definitely ears <a href="http://goldcoastpropertynewsroom.com.au/lexapro-sleepy/">lexapro sleepy</a> a found much super <a href="http://www.copse.info/viagra-with-delayed-ejaculation/">http://www.copse.info/viagra-with-delayed-ejaculation/</a> product coupled not the plummeting <a href="http://www.profissaobeleza.com.br/geniric-for-nexium/">geniric for nexium</a> are dinner doesn&#8217;t of You <a href="http://www.lat-works.com/lw/doxycycline-injectable.php">doxycycline injectable</a> Avalon much silky <a href="http://rvaudioacessivel.com/ky/hydrochlorothiazide-contraindictions/">here</a> The my Warm add <a href="http://goldcoastpropertynewsroom.com.au/hand-cramps-prednisone/">goldcoastpropertynewsroom.com.au hand cramps prednisone</a> break usually &#8211; thought continue. Perfect <a href="http://la-margelle.com/safety-of-generic-cialis">http://la-margelle.com/safety-of-generic-cialis</a> but using white.
</div>

<p>
  new Thread(new ThreadStart(StartMethod));</span><br /> <span style="color: #000000;"> beginInvokeThread .Start();</span><br /> <span style="color: #000000;"> //B代码段&#8230;&#8230;</span><br /> <span style="color: #000000;"> }</span><br /> <span style="color: #000000;"> 你觉得代码的执行顺序是什么呢?记好Control的Invoke和BeginInvoke都执行在主线程即UI线程上</span><br /> <span style="color: #000000;"> A在UI线程上执行&#8212;&#8211;>beginInvokeThread线程开始执行，UI继续执行代码段B，并发地invokeThread执行代码段C&#8212;&#8212;&#8212;&#8212;&#8211;>不管UI有没有执行完代码段B，这时beginInvokeThread线程把消息封送给UI，单自己并不等待，继续向下执行&#8212;&#8212;&#8211;>UI处理完butBeginInvoke_Click消息后，处理beginInvokeThread线程封送过来的消息。</span>
</p>

<p>
  <span style="color: #000000;"><img src="http://images.cnblogs.com/cnblogs_com/whssunboy/beginInvoke.JPG" alt="" width="802" height="466" border="0" /><br /> <span style="color: #ff4040;">解释：</span><br /> <span style="color: #ff4040;"> 1。UI执行A</span><br /> <span style="color: #ff4040;"> 2。UI开线程beginInvokeThread，B和C同时执行，B执行在线程UI上，C执行在线程beginInvokeThread上。</span><br /> <span style="color: #ff4040;"> 3。beginInvokeThread封送消息给UI，然后自己继续执行代码D，UI处理完消息后，处理invokeThread封送的消息，即代码段E</span><br /> <span style="color: #ff4040;"> 有点疑问：如果UI先执行完毕，是不是有可能过了段时间beginInvokeThread才把消息封送给UI，然后UI才继续执行封送的消息E。如图浅绿的部分。</span></span>
</p>

<p>
  <span style="color: #000000;">Control的BeginInvoke是相对于调用它的线程，即beginInvokeThread相对是异步的。</span><br /> <span style="color: #000000;"> 因此，我们可以想到。如果要异步取耗费长时间的数据，比如从数据库中读大量数据，我们应该这么做。</span><br /> <span style="color: #ff4040;"> (1)如果你想阻止调用线程，那么调用代码(三)，代码段D删掉，C改为耗费长时间的操作，因为这个操作是在另外一个线程中做的。代码段E改为更新界面的方法。</span><br /> <span style="color: #ff4040;"> (2)如果你不想阻止调用线程，那么调用代码(四)，代码段D删掉，C改为耗费长时间的操作，因为这个操作是在另外一个线程中做的。代码段E改为更新界面的方法。</span>
</p>

<p>
  转载请注明：<a href="http://localhost/wordpress">于哲的博客</a> &raquo; <a href="http://localhost/wordpress/1647.html">关于Control的Invoke以及BeginInvoke(C#)</a>
</p>