---
title: 【Greenlet】greenlet:轻量级并发编程
author: Flowerowl
layout: post
permalink: /3239.html
duoshuo_thread_id:
  - 1220743779864322532
views:
  - 491
bot_views:
  - 2
categories:
  - python
  - 技术杂谈
tags:
  - greenlet
  - python
  - thread
  - twisted
  - 协程
  - 进程
---
<div id="post-112" class="post">
  译者:gashero</p> <div class="entry">
    <p>
      目录
    </p>
    
    <ul>
      <li>
        <a name="id11" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id2"></a>1   动机 <ul>
          <li>
            <a name="id12" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id3"></a>1.1   例子
          </li>
        </ul>
      </li>
      
      <li>
        <a name="id13" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id4"></a>2   使用 <ul>
          <li>
            <a name="id14" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id5"></a>2.1   简介
          </li>
          <li>
            <a name="id15" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id6"></a>2.2   父greenlet
          </li>
          <li>
            <a name="id16" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id7"></a>2.3   实例
          </li>
          <li>
            <a name="id17" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id8"></a>2.4   切换
          </li>
          <li>
            <a name="id18" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id9"></a>2.5   greenlet的方法和属性
          </li>
          <li>
            <a name="id19" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#greenlet-python"></a>2.6   Greenlet与Python线程
          </li>
          <li>
            <a name="id20" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id10"></a>2.7   活动greenlet的垃圾收集
          </li>
        </ul>
      </li>
    </ul>
    
    <p>
       
    </p>
    
    <h1>
      <a name="id2" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id11"></a>1   动机
    </h1>
    
    <p>
      <strong>greenlet</strong> 包是 <strong>Stackless</strong> 的副产品，其将微线程称为 “tasklet” 。tasklet运行在伪并发中，使用channel进行同步数据交换。
    </p>
    
    <p>
      一个”greenlet”，是一个更加原始的微线程的概念，但是没有调度，或者叫做协程。这在你需要控制你的代码时很有用。你可以自己构造微线程的 调度器；也可以使用”greenlet”实现高级的控制流。例如可以重新创建构造器；不同于Python的构造器，我们的构造器可以嵌套的调用函数，而被 嵌套的函数也可以 yield 一个值。(另外，你并不需要一个”yield”关键字，参考例子)。
    </p>
    
    <p>
      Greenlet是作为一个C扩展模块给未修改的解释器的。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id3" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id12"></a>1.1   例子
    </h2>
    
    <p>
      假设系统是被控制台程序控制的，由用户输入命令。假设输入是一个个字符的。这样的系统有如如下的样子:
    </p>
    
    <pre>def process_commands(*args):
    while True:
        line=''
        while not line.endswith('n'):
            line+=read_next_char()
        if line=='quitn':
            print "are you sure?"
            if read_next_char()!="y":
                continue    #忽略指令
        process_commands(line)</pre>
    
    <p>
      现在假设你要把程序移植到GUI，而大多数GUI是事件驱动的。他们会在每次的用户输入时调用回调函数。这种情况下，就很难实现 read_next_char() 函数。我们有两个不兼容的函数:
    </p>
    
    <pre>def event_keydown(key):
    ??

def read_next_char():
    ?? 需要等待 event_keydown() 的调用</pre>
    
    <p>
      你可能在考虑用线程实现。而 Greenlet 是另一种解决方案，没有锁和关闭问题。你启动 process_commands() 函数，分割成 greenlet ，然后与按键事件交互，有如:
    </p>
    
    <pre>def event_keydown(key):
    g_processor.switch(key)

def read_next_char():
    g_self=greenlet.getcurrent()
    next_char=g_self.parent.switch()    #跳到上一层(main)的greenlet，等待下一次按键
    return next_char

g_processor=greenlet(process_commands)
g_processor.switch(*args)
gui.mainloop()</pre>
    
    <p>
      这个例子的执行流程是： read_next_char() 被调用，也就是 g_processor 的一部分，它就会切换(switch)到他的父greenlet，并假设继续在顶级主循环中执行(GUI主循环)。当GUI调用 event_keydown() 时，它切换到 g_processor ，这意味着执行会跳回到原来挂起的地方，也就是 read_next_char() 函数中的切换指令那里。然后 event_keydown() 的 key 参数就会被传递到 read_next_char() 的切换处，并返回。
    </p>
    
    <p>
      注意 read_next_char() 会被挂起并假设其调用栈会在恢复时保护的很好，所以他会在被调用的地方返回。这允许程序逻辑保持优美的顺序流。我们无需重写 process_commands() 来用到一个状态机中。
    </p>
    
    <p>
       
    </p>
    
    <h1>
      <a name="id4" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id13"></a>2   使用
    </h1>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id5" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id14"></a>2.1   简介
    </h2>
    
    <p>
      一个 “greenlet” 是一个很小的独立微线程。可以把它想像成一个堆栈帧，栈底是初始调用，而栈顶是当前greenlet的暂停位置。你使用greenlet创建一堆这样的堆 栈，然后在他们之间跳转执行。跳转不是绝对的：一个greenlet必须选择跳转到选择好的另一个greenlet，这会让前一个挂起，而后一个恢复。两 个greenlet之间的跳转称为 <strong>切换(switch)</strong> 。
    </p>
    
    <p>
      当你创建一个greenlet，它得到一个初始化过的空堆栈；当你第一次切换到它，他会启动指定的函数，然后切换跳出greenlet。当最终栈底 函数结束时，greenlet的堆栈又编程空的了，而greenlet也就死掉了。greenlet也会因为一个未捕捉的异常死掉。
    </p>
    
    <p>
      例如:
    </p>
    
    <pre>from py.magic import greenlet

def test1():
    print 12
    gr2.switch()
    print 34

def test2():
    print 56
    gr1.switch()
    print 78

gr1=greenlet(test1)
gr2=greenlet(test2)
gr1.switch()</pre>
    
    <p>
      最后一行跳转到 test1() ，它打印12，然后跳转到 test2() ，打印56，然后跳转回 test1() ，打印34，然后 test1() 就结束，gr1死掉。这时执行会回到原来的 gr1.switch() 调用。注意，78是不会被打印的。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id6" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id15"></a>2.2   父greenlet
    </h2>
    
    <p>
      现在看看一个greenlet死掉时执行点去哪里。每个greenlet拥有一个父greenlet。父greenlet在每个greenlet初 始化时被创建(不过可以在任何时候改变)。父greenlet是当greenlet死掉时，继续原来的位置执行。这样，greenlet就被组织成一棵 树，顶级的代码并不在用户创建的 greenlet 中运行，而称为主greenlet，也就是树根。
    </p>
    
    <p>
      在上面的例子中，gr1和gr2都是把主greenlet作为父greenlet的。任何一个死掉，执行点都会回到主函数。
    </p>
    
    <p>
      未捕获的异常会波及到父greenlet。如果上面的 test2() 包含一个打印错误(typo)，他会生成一个 NameError 而干掉gr2，然后执行点会回到主函数。traceback会显示 test2() 而不是 test1() 。记住，切换不是调用，但是执行点可以在并行的栈容器间并行交换，而父greenlet定义了栈最初从哪里来。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id7" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id16"></a>2.3   实例
    </h2>
    
    <p>
      <strong>py.magic.greenlet</strong> 是一个 greenlet 类型，支持如下操作：
    </p>
    
    <p>
      <tt>greenlet(run=None,parent=None)</tt>
    </p>
    
    <blockquote>
      <p>
        创建一个greenlet对象，而不执行。run是执行回调，而parent是父greenlet，缺省是当前greenlet。
      </p>
    </blockquote>
    
    <p>
      <tt>greenlet.getcurrent()</tt>
    </p>
    
    <blockquote>
      <p>
        返回当前greenlet，也就是谁在调用这个函数。
      </p>
    </blockquote>
    
    <p>
      <tt>greenlet.GreenletExit</tt>
    </p>
    
    <blockquote>
      <p>
        这个特定的异常不会波及到父greenlet，它用于干掉一个greenlet。
      </p>
    </blockquote>
    
    <p>
      greenlet 类型可以被继承。一个greenlet通过调用其 run 属性执行，就是创建时指定的那个。对于子类，可以定义一个 run() 方法，而不必严格遵守在构造器中给出 run 参数。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id8" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id17"></a>2.4   切换
    </h2>
    
    <p>
      greenlet之间的切换发生在greenlet的 switch() 方法被调用时，这会让执行点跳转到greenlet的 switch() 被调用处。或者在greenlet死掉时，跳转到父greenlet那里去。在切换时，一个对象或异常被发送到目标greenlet。这可以作为两个greenlet之间传递信息的方便方式。例如:
    </p>
    
    <pre>def test1(x,y):
    z=gr2.switch(x+y)
    print z

def test2(u):
    print u
    gr1.switch(42)

gr1=greenlet(test1)
gr2=greenlet(test2)
gr1.switch("hello"," world")</pre>
    
    <p>
      这会打印出 “hello world” 和42，跟前面的例子的输出顺序相同。注意 test1() 和 test2() 的参数并不是在 greenlet 创建时指定的，而是在第一次切换到这里时传递的。
    </p>
    
    <p>
      这里是精确的调用方式:
    </p>
    
    <pre>g.switch(obj=None or *args)</pre>
    
    <p>
      切换到执行点greenlet g，发送给定的对象obj。在特殊情况下，如果g还没有启动，就会让它启动；这种情况下，会传递参数过去，然后调用 <tt>g.run(*args)</tt> 。
    </p>
    
    <p>
      <strong>垂死的greenlet</strong>
    </p>
    
    <blockquote>
      <p>
        如果一个greenlet的 run() 结束了，他会返回值到父greenlet。如果 run() 是异常终止的，异常会波及到父greenlet(除非是 greenlet.GreenletExit 异常，这种情况下异常会被捕捉并返回到父greenlet)。
      </p>
    </blockquote>
    
    <p>
      除了上面的情况外，目标greenlet会接收到发送来的对象作为 switch() 的返回值。虽然 switch() 并不会立即返回，但是它仍然会在未来某一点上返回，当其他greenlet切换回来时。当这发生时，执行点恢复到 switch() 之后，而 switch() 返回刚才调用者发送来的对象。这意味着 <tt>x=g.switch(y)</tt> 会发送对象y到g，然后等着一个不知道是谁发来的对象，并在这里返回给x。
    </p>
    
    <p>
      注意，任何尝试切换到死掉的greenlet的行为都会切换到死掉greenlet的父greenlet，或者父的父，等等。最终的父就是 main greenlet，永远不会死掉的。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id9" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id18"></a>2.5   greenlet的方法和属性
    </h2>
    
    <p>
      <tt>g.switch(obj=None or *args)</tt>
    </p>
    
    <blockquote>
      <p>
        切换执行点到greenlet g，同上。
      </p>
    </blockquote>
    
    <p>
      <tt>g.run</tt>
    </p>
    
    <blockquote>
      <p>
        调用可执行的g，并启动。在g启动后，这个属性就不再存在了。
      </p>
    </blockquote>
    
    <p>
      <tt>g.parent</tt>
    </p>
    
    <blockquote>
      <p>
        greenlet的父。这是可写的，但是不允许创建循环的父关系。
      </p>
    </blockquote>
    
    <p>
      <tt>g.gr_frame</tt>
    </p>
    
    <blockquote>
      <p>
        当前顶级帧，或者None。
      </p>
    </blockquote>
    
    <p>
      <tt>g.dead</tt>
    </p>
    
    <blockquote>
      <p>
        判断是否已经死掉了
      </p>
    </blockquote>
    
    <p>
      <tt>bool(g)</tt>
    </p>
    
    <blockquote>
      <p>
        如果g是活跃的则返回True，在尚未启动或者结束后返回False。
      </p>
    </blockquote>
    
    <p>
      <tt>g.throw([typ,[val,[tb]]])</tt>
    </p>
    
    <blockquote>
      <p>
        切换执行点到greenlet g，但是立即抛出指定的异常到g。如果没有提供参数，异常缺省就是 greenlet.GreenletExit 。根据异常波及规则，有如上面描述的。注意调用这个方法等同于如下:
      </p>
      
      <pre>def raiser():
    raise typ,val,tb

g_raiser=greenlet(raiser,parent=g)
g_raiser.switch()</pre>
    </blockquote>
    
    <p>
       
    </p>
    
    <h2>
      <a name="greenlet-python" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id19"></a>2.6   Greenlet与Python线程
    </h2>
    
    <p>
      greenlet可以与Python线程一起使用；在这种情况下，每个线程包含一个独立的 main greenlet，并拥有自己的greenlet树。不同线程之间不可以互相切换greenlet。
    </p>
    
    <p>
       
    </p>
    
    <h2>
      <a name="id10" href="///E:/gasprj/gmnotes/doc-rest/py_mod/greenlet_programming.rst.html#id20"></a>2.7   活动greenlet的垃圾收集
    </h2>
    
    <p>
      如果不再有对greenlet对象的引用时(包括其他greenlet的parent)，还是没有办法切换回greenlet。这种情况下会生成一个 GreenletExit 异常到greenlet。这是greenlet收到异步异常的唯一情况。应该给出一个 try .. finally 用于清理greenlet内的资源。这个功能同时允许greenlet中无限循环的编程风格。这样循环可以在最后一个引用消失时自动中断。
    </p>
    
    <p>
      如果不希望greenlet死掉或者把引用放到别处，只需要捕捉和忽略 GreenletExit 异常即可。
    </p>
    
    <p>
      greenlet不参与垃圾收集；greenlet帧的循环引用数据会被检测到。将引用传递到其他的循环greenlet会引起内存泄露。
    </p>
    
    <p class="postmetadata alt">
       
    </p>
  </div>
</div>

转载请注明：[于哲的博客][1] &raquo; [【Greenlet】greenlet:轻量级并发编程][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3239.html