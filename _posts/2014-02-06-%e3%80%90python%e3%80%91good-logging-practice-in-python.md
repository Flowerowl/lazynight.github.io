---
title: 【Python】Good logging practice in Python
author: Flowerowl
layout: post
permalink: /3372.html
views:
  - 429
duoshuo_thread_id:
  - 1220743779864322563
categories:
  - python
tags:
  - log
  - logging
  - python
---
<div class="post-content">
  <p>
    In reality, logging is important. When you transfer money, there are transfer records. When an airplane is flying, black box (flight data recorder) is recording everything. If something goes wrong, people can read the log and has a chance to figure out what happened. Likewise, logging is important for system developing, debugging and running. When a program crashes, if there is no logging record, you have little chance to understand what happened. For example, when you are writing a server, logging is necessary. Following screenshot is the log file of a <a href="http://ezcomet.com">EZComet.com</a> server.
  </p>
  
  <p>
    <img title="ziCqzyTTlWY38sTiog0U_ezcomet_logging.png" src="http://lazynight.me/wp-content/uploads/2014/02/ziCqzyTTlWY38sTiog0U_ezcomet_logging.png" alt="ZiCqzyTTlWY38sTiog0U ezcomet logging" width="600" height="376" border="0" />
  </p>
  
  <p>
    Without the log, I can hardly know what’s wrong if a service goes down. Not only for the servers, logging is also important for desktop GUI applications. For instance, when your program crashes on your customer’s PC, you can ask them to send the log files to you, and you may can figure why. Trust me, you will never know what kind of strange issues there will be in different PC environments. I once received an error log report like this
  </p>
  
  <div class="highlight">
    <pre><span class="mi">2011</span><span class="o">-</span><span class="mi">08</span><span class="o">-</span><span class="mi">22</span> <span class="mi">17</span><span class="p">:</span><span class="mi">52</span><span class="p">:</span><span class="mi">54</span><span class="p">,</span><span class="mi">828</span> <span class="o">-</span> <span class="n">root</span> <span class="o">-</span> <span class="n">ERROR</span> <span class="o">-</span> <span class="p">[</span><span class="n">Errno</span> <span class="mi">10104</span><span class="p">]</span> <span class="n">getaddrinfo</span> <span class="n">failed</span>
<span class="n">Traceback</span> <span class="p">(</span><span class="n">most</span> <span class="n">recent</span> <span class="n">call</span> <span class="n">last</span><span class="p">):</span>
  <span class="n">File</span> <span class="s">"&lt;string&gt;"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">124</span><span class="p">,</span> <span class="ow">in</span> <span class="n">main</span>
  <span class="n">File</span> <span class="s">"&lt;string&gt;"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">20</span><span class="p">,</span> <span class="ow">in</span> <span class="n">__init__</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/wx._core"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">7978</span><span class="p">,</span> <span class="ow">in</span> <span class="n">__init__</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/wx._core"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">7552</span><span class="p">,</span> <span class="ow">in</span> <span class="n">_BootstrapApp</span>
  <span class="n">File</span> <span class="s">"&lt;string&gt;"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">84</span><span class="p">,</span> <span class="ow">in</span> <span class="n">OnInit</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.wxreactor"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">175</span><span class="p">,</span> <span class="ow">in</span> <span class="n">install</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet._threadedselect"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">106</span><span class="p">,</span> <span class="ow">in</span> <span class="n">__init__</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.base"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">488</span><span class="p">,</span> <span class="ow">in</span> <span class="n">__init__</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.posixbase"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">266</span><span class="p">,</span> <span class="ow">in</span> <span class="n">installWaker</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/twisted.internet.posixbase"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">74</span><span class="p">,</span> <span class="ow">in</span> <span class="n">__init__</span>
  <span class="n">File</span> <span class="s">"h:\workspace\project</span><span class="se">\b</span><span class="s">uild\pyi.win32\mrdj\outPYZ1.pyz/socket"</span><span class="p">,</span> <span class="n">line</span> <span class="mi">224</span><span class="p">,</span> <span class="ow">in</span> <span class="n">meth</span>
<span class="n">gaierror</span><span class="p">:</span> <span class="p">[</span><span class="n">Errno</span> <span class="mi">10104</span><span class="p">]</span> <span class="n">getaddrinfo</span> <span class="n">failed</span>
</pre>
  </div>
  
  <p>
    And eventually figure out that the customer PC is infected by a virus which makes call to gethostname failed. See, how can you even know this if there is no log to read?
  </p>
  
  <h3>
    print is not a good idea
  </h3>
  
  <p>
    Although logging is important, not all developers know how to use them correctly. I saw some developers insert print statements when developing and remove those statements when it is finished. It may looks like this
  </p>
  
  <div class="highlight">
    <pre><span class="k">print</span> <span class="s">'Start reading database'</span>
<span class="n">records</span> <span class="o">=</span> <span class="n">model</span><span class="o">.</span><span class="n">read_recrods</span><span class="p">()</span>
<span class="k">print</span> <span class="s">'# records'</span><span class="p">,</span> <span class="n">records</span>
<span class="k">print</span> <span class="s">'Updating record ...'</span>
<span class="n">model</span><span class="o">.</span><span class="n">update_records</span><span class="p">(</span><span class="n">records</span><span class="p">)</span>
<span class="k">print</span> <span class="s">'done'</span>
</pre>
  </div>
  
  <p>
    It works when the program is a simple script, but for complex systems, you better not to use this approach. First of all, you cannot leave only important messages in the log, you may see a lots of garbage messages in the log, but can’t find anything useful.You also cannot control those print statements without modifying code, you may forgot to remove those unused prints. And all printed messages go into stdout, which is bad when you have data to output to stdout. Of course you can print messages to stderr, but still, it is not a good practice to use print for logging.
  </p>
  
  <h3>
    Use Python standard logging module
  </h3>
  
  <p>
    So, how do you do logging correctly? It’s easy, use the standard Python logging module. Thanks to Python community, logging is a standard module, it was well designed to be easy-to-use and very flexible. You can use the <a href="http://docs.python.org/2/library/logging">logging system</a> like this
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>
<span class="n">logging</span><span class="o">.</span><span class="n">basicConfig</span><span class="p">(</span><span class="n">level</span><span class="o">=</span><span class="n">logging</span><span class="o">.</span><span class="n">INFO</span><span class="p">)</span>
<span class="n">logger</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>

<span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Start reading database'</span><span class="p">)</span>
<span class="c"># read database here</span>
<span class="n">records</span> <span class="o">=</span> <span class="p">{</span><span class="s">'john'</span><span class="p">:</span> <span class="mi">55</span><span class="p">,</span> <span class="s">'tom'</span><span class="p">:</span> <span class="mi">66</span><span class="p">}</span>
<span class="n">logger</span><span class="o">.</span><span class="n">debug</span><span class="p">(</span><span class="s">'Records: </span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">records</span><span class="p">)</span>
<span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Updating records ...'</span><span class="p">)</span>
<span class="c"># update records here</span>
<span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Finish updating records'</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    You can run it and see
  </p>
  
  <div class="highlight">
    <pre><span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Start</span> <span class="n">reading</span> <span class="n">database</span>
<span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Updating</span> <span class="n">records</span> <span class="o">...</span>
<span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Finish</span> <span class="n">updating</span> <span class="n">records</span>
</pre>
  </div>
  
  <p>
    What’s different between the “print” approach you asked. Well, of course there are benefits:
  </p>
  
  <ul>
    <li>
      You can control message level and filter out not important ones
    </li>
    <li>
      You can decide where and how to output later
    </li>
  </ul>
  
  <p>
    There are different importance levels you can use, debug, info, warning, error and critical. By giving different level to logger or handler, you can write only error messages to specific log file, or record debug details when debugging. Let’s change the logger level to DEBUG and see the output again
  </p>
  
  <div class="highlight">
    <pre><span class="n">logging</span><span class="o">.</span><span class="n">basicConfig</span><span class="p">(</span><span class="n">level</span><span class="o">=</span><span class="n">logging</span><span class="o">.</span><span class="n">DEBUG</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    The output:
  </p>
  
  <div class="highlight">
    <pre><span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Start</span> <span class="n">reading</span> <span class="n">database</span>
<span class="n">DEBUG</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Records</span><span class="o">:</span> <span class="o">{</span><span class="s1">'john'</span><span class="o">:</span> <span class="mi">55</span><span class="o">,</span> <span class="s1">'tom'</span><span class="o">:</span> <span class="mi">66</span><span class="o">}</span>
<span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Updating</span> <span class="n">records</span> <span class="o">...</span>
<span class="n">INFO</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Finish</span> <span class="n">updating</span> <span class="n">records</span>
</pre>
  </div>
  
  <p>
    As you can see, we adjust the logger level to DEBUG, then debug records appear in output. You can also decide how these messages are processed. For example, you can use a FileHandler to write records to a file.
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>

<span class="n">logger</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>
<span class="n">logger</span><span class="o">.</span><span class="n">setLevel</span><span class="p">(</span><span class="n">logging</span><span class="o">.</span><span class="n">INFO</span><span class="p">)</span>

<span class="c"># create a file handler</span>
<span class="n">handler</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">FileHandler</span><span class="p">(</span><span class="s">'hello.log'</span><span class="p">)</span>
<span class="n">handler</span><span class="o">.</span><span class="n">setLevel</span><span class="p">(</span><span class="n">logging</span><span class="o">.</span><span class="n">INFO</span><span class="p">)</span>

<span class="c"># create a logging format</span>
<span class="n">formatter</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">Formatter</span><span class="p">(</span><span class="s">'</span><span class="si">%(asctime)s</span><span class="s"> - </span><span class="si">%(name)s</span><span class="s"> - </span><span class="si">%(levelname)s</span><span class="s"> - </span><span class="si">%(message)s</span><span class="s">'</span><span class="p">)</span>
<span class="n">handler</span><span class="o">.</span><span class="n">setFormatter</span><span class="p">(</span><span class="n">formatter</span><span class="p">)</span>

<span class="c"># add the handlers to the logger</span>
<span class="n">logger</span><span class="o">.</span><span class="n">addHandler</span><span class="p">(</span><span class="n">handler</span><span class="p">)</span>

<span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Hello baby'</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    There are different handlers, you can also send records to you mailbox or even a to a remote server. You can also write your own custom logging handler. I’m not going to tell you details, please reference to official documents: <a href="http://docs.python.org/2/howto/logging.html#logging-basic-tutorial">Basic Tutorial</a>, <a href="http://docs.python.org/2/howto/logging.html#logging-advanced-tutorial">Advanced Tutorial</a> and <a href="http://docs.python.org/2/howto/logging-cookbook.html#logging-cookbook">Logging Cookbook</a>.
  </p>
  
  <h3>
    Write logging records everywhere with proper level
  </h3>
  
  <p>
    With flexibility of the logging module, you can write logging record everywhere with proper level and configure them later. What is the proper level to use, you may ask. Here I share my experience.
  </p>
  
  <p>
    In most cases, you don’t want to read too much details in the log file. Therefore, debug level is only enabled when you are debugging. I use debug level only for detail debugging information, especially when the data is big or the frequency is high, such as records of algorithm internal state changes in a for-loop.
  </p>
  
  <div class="highlight">
    <pre><span class="k">def</span> <span class="nf">complex_algorithm</span><span class="p">(</span><span class="n">items</span><span class="p">):</span>
    <span class="k">for</span> <span class="n">i</span><span class="p">,</span> <span class="n">item</span> <span class="ow">in</span> <span class="nb">enumerate</span><span class="p">(</span><span class="n">items</span><span class="p">):</span>
        <span class="c"># do some complex algorithm computation</span>
        <span class="n">logger</span><span class="o">.</span><span class="n">debug</span><span class="p">(</span><span class="s">'</span><span class="si">%s</span><span class="s"> iteration, item=</span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">i</span><span class="p">,</span> <span class="n">item</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    I use info level for routines, for example, handling requests or server state changed.
  </p>
  
  <div class="highlight">
    <pre><span class="k">def</span> <span class="nf">handle_request</span><span class="p">(</span><span class="n">request</span><span class="p">):</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Handling request </span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">request</span><span class="p">)</span>
    <span class="c"># handle request here</span>
    <span class="n">result</span> <span class="o">=</span> <span class="s">'result'</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Return result: </span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">result</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">start_service</span><span class="p">():</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Starting service at port </span><span class="si">%s</span><span class="s"> ...'</span><span class="p">,</span> <span class="n">port</span><span class="p">)</span>
    <span class="n">service</span><span class="o">.</span><span class="n">start</span><span class="p">()</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Service is started'</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    I use warning when it is important, but not an error, for example, when a user attempts to login with wrong password or connection is slow.
  </p>
  
  <div class="highlight">
    <pre><span class="k">def</span> <span class="nf">authenticate</span><span class="p">(</span><span class="n">user_name</span><span class="p">,</span> <span class="n">password</span><span class="p">,</span> <span class="n">ip_address</span><span class="p">):</span>
    <span class="k">if</span> <span class="n">user_name</span> <span class="o">!=</span> <span class="n">USER_NAME</span> <span class="ow">and</span> <span class="n">password</span> <span class="o">!=</span> <span class="n">PASSWORD</span><span class="p">:</span>
        <span class="n">logger</span><span class="o">.</span><span class="n">warn</span><span class="p">(</span><span class="s">'Login attempt to </span><span class="si">%s</span><span class="s"> from IP </span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">user_name</span><span class="p">,</span> <span class="n">ip_address</span><span class="p">)</span>
        <span class="k">return</span> <span class="bp">False</span>
    <span class="c"># do authentication here</span>
</pre>
  </div>
  
  <p>
    I use error level when something is wrong, for example, an exception is thrown, IO operation failure or connectivity issue.
  </p>
  
  <div class="highlight">
    <pre><span class="k">def</span> <span class="nf">get_user_by_id</span><span class="p">(</span><span class="n">user_id</span><span class="p">):</span>
    <span class="n">user</span> <span class="o">=</span> <span class="n">db</span><span class="o">.</span><span class="n">read_user</span><span class="p">(</span><span class="n">user_id</span><span class="p">)</span>
    <span class="k">if</span> <span class="n">user</span> <span class="ow">is</span> <span class="bp">None</span><span class="p">:</span>
        <span class="n">logger</span><span class="o">.</span><span class="n">error</span><span class="p">(</span><span class="s">'Cannot find user with user_id=</span><span class="si">%s</span><span class="s">'</span><span class="p">,</span> <span class="n">user_id</span><span class="p">)</span>
        <span class="k">return</span> <span class="n">user</span>
    <span class="k">return</span> <span class="n">user</span>
</pre>
  </div>
  
  <p>
    I seldom use critical, you can use it when something really bad happen, for example, out of memory, disk is full or a nuclear meltdown (Hope that will not happen :S).
  </p>
  
  <h3>
    Use __name__ as the logger name
  </h3>
  
  <p>
    You don’t have to set the logger name as __name__, but by doing that, it brings us some benefits. The variable __name__ is current module name in Python. For example, you call logger.getLogger(__name__) in a module “foo.bar.my_module”, then it is logger.getLogger(“foo.bar.my_module”). When you need to configure the logger, you can configure to “foo”, then all modules in “foo” packages shares same configuration. You can also understand what is the module of message when reading the log.
  </p>
  
  <h3>
    Capture exceptions and record them with traceback
  </h3>
  
  <p>
    It is always a good practice to record when something goes wrong, but it won’t be helpful if there is no traceback. You should capture exceptions and record them with traceback. Following is an example:
  </p>
  
  <div class="highlight">
    <pre><span class="k">try</span><span class="p">:</span>
    <span class="nb">open</span><span class="p">(</span><span class="s">'/path/to/does/not/exist'</span><span class="p">,</span> <span class="s">'rb'</span><span class="p">)</span>
<span class="k">except</span> <span class="p">(</span><span class="ne">SystemExit</span><span class="p">,</span> <span class="ne">KeyboardInterrupt</span><span class="p">):</span>
    <span class="k">raise</span>
<span class="k">except</span> <span class="ne">Exception</span><span class="p">,</span> <span class="n">e</span><span class="p">:</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">error</span><span class="p">(</span><span class="s">'Failed to open file'</span><span class="p">,</span> <span class="n">exc_info</span><span class="o">=</span><span class="bp">True</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    By calling logger methods with exc_info=True parameter, traceback is dumped to the logger. As you can see the result
  </p>
  
  <div class="highlight">
    <pre><span class="n">ERROR</span><span class="o">:</span><span class="n">__main__</span><span class="o">:</span><span class="n">Failed</span> <span class="n">to</span> <span class="n">open</span> <span class="n">file</span>
<span class="n">Traceback</span> <span class="o">(</span><span class="n">most</span> <span class="n">recent</span> <span class="n">call</span> <span class="n">last</span><span class="o">):</span>
  <span class="n">File</span> <span class="s2">"example.py"</span><span class="o">,</span> <span class="n">line</span> <span class="mi">6</span><span class="o">,</span> <span class="k">in</span> <span class="o">&lt;</span><span class="n">module</span><span class="o">&gt;</span>
    <span class="n">open</span><span class="o">(</span><span class="s1">'/path/to/does/not/exist'</span><span class="o">,</span> <span class="s1">'rb'</span><span class="o">)</span>
<span class="n">IOError</span><span class="o">:</span> <span class="o">[</span><span class="n">Errno</span> <span class="mi">2</span><span class="o">]</span> <span class="n">No</span> <span class="n">such</span> <span class="n">file</span> <span class="n">or</span> <span class="n">directory</span><span class="o">:</span> <span class="s1">'/path/to/does/not/exist'</span>
</pre>
  </div>
  
  <p>
    You can also call logger.exception(msg, *args), it equals to logger.error(msg, exc_info=True, *args).
  </p>
  
  <h3>
    Do not get logger at the module level unless disable_existing_loggers is False
  </h3>
  
  <p>
    You can see a lots of example out there (including this article, I did it just for giving example in short) get logger at module level. They looks harmless, but actually, there is a pitfall – Python logging module respects all created logger before you load the configuration from a file, if you get logger at the module level like this
  </p>
  
  <p>
    my_module.py
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>

<span class="n">logger</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>

<span class="k">def</span> <span class="nf">foo</span><span class="p">():</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Hi, foo'</span><span class="p">)</span>

<span class="k">class</span> <span class="nc">Bar</span><span class="p">(</span><span class="nb">object</span><span class="p">):</span>
    <span class="k">def</span> <span class="nf">bar</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Hi, bar'</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    main.py
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>

<span class="c"># load my module</span>
<span class="kn">import</span> <span class="nn">my_module</span>

<span class="c"># load the logging configuration</span>
<span class="n">logging</span><span class="o">.</span><span class="n">config</span><span class="o">.</span><span class="n">fileConfig</span><span class="p">(</span><span class="s">'logging.ini'</span><span class="p">)</span>

<span class="n">my_module</span><span class="o">.</span><span class="n">foo</span><span class="p">()</span>
<span class="n">bar</span> <span class="o">=</span> <span class="n">my_module</span><span class="o">.</span><span class="n">Bar</span><span class="p">()</span>
<span class="n">bar</span><span class="o">.</span><span class="n">bar</span><span class="p">()</span>
</pre>
  </div>
  
  <p>
    logging.ini
  </p>
  
  <div class="highlight">
    <pre><span class="p">[</span><span class="n">loggers</span><span class="p">]</span>
<span class="n">keys</span><span class="o">=</span><span class="n">root</span>

<span class="p">[</span><span class="n">handlers</span><span class="p">]</span>
<span class="n">keys</span><span class="o">=</span><span class="n">consoleHandler</span>

<span class="p">[</span><span class="n">formatters</span><span class="p">]</span>
<span class="n">keys</span><span class="o">=</span><span class="n">simpleFormatter</span>

<span class="p">[</span><span class="n">logger_root</span><span class="p">]</span>
<span class="n">level</span><span class="o">=</span><span class="n">DEBUG</span>
<span class="n">handlers</span><span class="o">=</span><span class="n">consoleHandler</span>

<span class="p">[</span><span class="n">handler_consoleHandler</span><span class="p">]</span>
<span class="n">class</span><span class="o">=</span><span class="n">StreamHandler</span>
<span class="n">level</span><span class="o">=</span><span class="n">DEBUG</span>
<span class="n">formatter</span><span class="o">=</span><span class="n">simpleFormatter</span>
<span class="n">args</span><span class="o">=</span><span class="p">(</span><span class="n">sys</span><span class="p">.</span><span class="n">stdout</span><span class="p">,)</span>

<span class="p">[</span><span class="n">formatter_simpleFormatter</span><span class="p">]</span>
<span class="n">format</span><span class="o">=%</span><span class="p">(</span><span class="n">asctime</span><span class="p">)</span><span class="n">s</span> <span class="o">-</span> <span class="o">%</span><span class="p">(</span><span class="n">name</span><span class="p">)</span><span class="n">s</span> <span class="o">-</span> <span class="o">%</span><span class="p">(</span><span class="n">levelname</span><span class="p">)</span><span class="n">s</span> <span class="o">-</span> <span class="o">%</span><span class="p">(</span><span class="n">message</span><span class="p">)</span><span class="n">s</span>
<span class="n">datefmt</span><span class="o">=</span>
</pre>
  </div>
  
  <p>
    And you expect to see the records appear in log, but you will see nothing. Why? Because you create the logger at module level, you then import the module before you load the logging configuration from a file. The logging.fileConfig and logging.dictConfig disables existing loggers by default. So, those setting in file will not be applied to your logger. It’s better to get the logger when you need it. It’s cheap to create or get a logger. You can write the code like this:
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>

<span class="k">def</span> <span class="nf">foo</span><span class="p">():</span>
    <span class="n">logger</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>
    <span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Hi, foo'</span><span class="p">)</span>

<span class="k">class</span> <span class="nc">Bar</span><span class="p">(</span><span class="nb">object</span><span class="p">):</span>
    <span class="k">def</span> <span class="nf">__init__</span><span class="p">(</span><span class="bp">self</span><span class="p">,</span> <span class="n">logger</span><span class="o">=</span><span class="bp">None</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">logger</span> <span class="o">=</span> <span class="n">logger</span> <span class="ow">or</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>

    <span class="k">def</span> <span class="nf">bar</span><span class="p">(</span><span class="bp">self</span><span class="p">):</span>
        <span class="bp">self</span><span class="o">.</span><span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'Hi, bar'</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    By doing that, the loggers will be created after you load the configuration. The setting will be applied correctly.
  </p>
  
  <p>
    Since Python2.7, a new argument name “disable_existing_loggers” to fileConfig and dictConfig (as a parameter in schema) is added, by setting it to False, problem mentioned above can be solved. For example:
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">logging</span>
<span class="kn">import</span> <span class="nn">logging.config</span>

<span class="n">logger</span> <span class="o">=</span> <span class="n">logging</span><span class="o">.</span><span class="n">getLogger</span><span class="p">(</span><span class="n">__name__</span><span class="p">)</span>

<span class="c"># load config from file </span>
<span class="c"># logging.config.fileConfig('logging.ini', disable_existing_loggers=False)</span>
<span class="c"># or, for dictConfig</span>
<span class="n">logging</span><span class="o">.</span><span class="n">config</span><span class="o">.</span><span class="n">dictConfig</span><span class="p">({</span>
    <span class="s">'version'</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span>
    <span class="s">'disable_existing_loggers'</span><span class="p">:</span> <span class="bp">False</span><span class="p">,</span>  <span class="c"># this fixes the problem</span>
    <span class="s">'formatters'</span><span class="p">:</span> <span class="p">{</span>
        <span class="s">'standard'</span><span class="p">:</span> <span class="p">{</span>
            <span class="s">'format'</span><span class="p">:</span> <span class="s">'</span><span class="si">%(asctime)s</span><span class="s"> [</span><span class="si">%(levelname)s</span><span class="s">] </span><span class="si">%(name)s</span><span class="s">: </span><span class="si">%(message)s</span><span class="s">'</span>
        <span class="p">},</span>
    <span class="p">},</span>
    <span class="s">'handlers'</span><span class="p">:</span> <span class="p">{</span>
        <span class="s">'default'</span><span class="p">:</span> <span class="p">{</span>
            <span class="s">'level'</span><span class="p">:</span><span class="s">'INFO'</span><span class="p">,</span>
            <span class="s">'class'</span><span class="p">:</span><span class="s">'logging.StreamHandler'</span><span class="p">,</span>
        <span class="p">},</span>
    <span class="p">},</span>
    <span class="s">'loggers'</span><span class="p">:</span> <span class="p">{</span>
        <span class="s">''</span><span class="p">:</span> <span class="p">{</span>
            <span class="s">'handlers'</span><span class="p">:</span> <span class="p">[</span><span class="s">'default'</span><span class="p">],</span>
            <span class="s">'level'</span><span class="p">:</span> <span class="s">'INFO'</span><span class="p">,</span>
            <span class="s">'propagate'</span><span class="p">:</span> <span class="bp">True</span>
        <span class="p">}</span>
    <span class="p">}</span>
<span class="p">})</span>

<span class="n">logger</span><span class="o">.</span><span class="n">info</span><span class="p">(</span><span class="s">'It works!'</span><span class="p">)</span>
</pre>
  </div>
  
  <h3>
    Use JSON or YAML logging configuration
  </h3>
  
  <p>
    You can configure your logging system in Python code, but it is not flexible. It’s better to use a logging configuration file. After Python 2.7, you can load logging configuration from a dict. It means you can load the logging configuration from a JSON or YAML file. Although you can use the old .ini style logging configuration, it is difficult to read and write. Here I show you an logging configuration example in JSON or YAML
  </p>
  
  <p>
    logging.json
  </p>
  
  <div class="highlight">
    <pre><span class="p">{</span>
    <span class="nt">"version"</span><span class="p">:</span> <span class="mi">1</span><span class="p">,</span>
    <span class="nt">"disable_existing_loggers"</span><span class="p">:</span> <span class="kc">false</span><span class="p">,</span>
    <span class="nt">"formatters"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"simple"</span><span class="p">:</span> <span class="p">{</span>
            <span class="nt">"format"</span><span class="p">:</span> <span class="s2">"%(asctime)s - %(name)s - %(levelname)s - %(message)s"</span>
        <span class="p">}</span>
    <span class="p">},</span>

    <span class="nt">"handlers"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"console"</span><span class="p">:</span> <span class="p">{</span>
            <span class="nt">"class"</span><span class="p">:</span> <span class="s2">"logging.StreamHandler"</span><span class="p">,</span>
            <span class="nt">"level"</span><span class="p">:</span> <span class="s2">"DEBUG"</span><span class="p">,</span>
            <span class="nt">"formatter"</span><span class="p">:</span> <span class="s2">"simple"</span><span class="p">,</span>
            <span class="nt">"stream"</span><span class="p">:</span> <span class="s2">"ext://sys.stdout"</span>
        <span class="p">},</span>

        <span class="nt">"info_file_handler"</span><span class="p">:</span> <span class="p">{</span>
            <span class="nt">"class"</span><span class="p">:</span> <span class="s2">"logging.handlers.RotatingFileHandler"</span><span class="p">,</span>
            <span class="nt">"level"</span><span class="p">:</span> <span class="s2">"INFO"</span><span class="p">,</span>
            <span class="nt">"formatter"</span><span class="p">:</span> <span class="s2">"simple"</span><span class="p">,</span>
            <span class="nt">"filename"</span><span class="p">:</span> <span class="s2">"info.log"</span><span class="p">,</span>
            <span class="nt">"maxBytes"</span><span class="p">:</span> <span class="s2">"10485760"</span><span class="p">,</span>
            <span class="nt">"backupCount"</span><span class="p">:</span> <span class="s2">"20"</span><span class="p">,</span>
            <span class="nt">"encoding"</span><span class="p">:</span> <span class="s2">"utf8"</span>
        <span class="p">},</span>

        <span class="nt">"error_file_handler"</span><span class="p">:</span> <span class="p">{</span>
            <span class="nt">"class"</span><span class="p">:</span> <span class="s2">"logging.handlers.RotatingFileHandler"</span><span class="p">,</span>
            <span class="nt">"level"</span><span class="p">:</span> <span class="s2">"ERROR"</span><span class="p">,</span>
            <span class="nt">"formatter"</span><span class="p">:</span> <span class="s2">"simple"</span><span class="p">,</span>
            <span class="nt">"filename"</span><span class="p">:</span> <span class="s2">"errors.log"</span><span class="p">,</span>
            <span class="nt">"maxBytes"</span><span class="p">:</span> <span class="s2">"10485760"</span><span class="p">,</span>
            <span class="nt">"backupCount"</span><span class="p">:</span> <span class="s2">"20"</span><span class="p">,</span>
            <span class="nt">"encoding"</span><span class="p">:</span> <span class="s2">"utf8"</span>
        <span class="p">}</span>
    <span class="p">},</span>

    <span class="nt">"loggers"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"my_module"</span><span class="p">:</span> <span class="p">{</span>
            <span class="nt">"level"</span><span class="p">:</span> <span class="s2">"ERROR"</span><span class="p">,</span>
            <span class="nt">"handlers"</span><span class="p">:</span> <span class="p">[</span><span class="s2">"console"</span><span class="p">],</span>
            <span class="nt">"propagate"</span><span class="p">:</span> <span class="s2">"no"</span>
        <span class="p">}</span>
    <span class="p">},</span>

    <span class="nt">"root"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"level"</span><span class="p">:</span> <span class="s2">"INFO"</span><span class="p">,</span>
        <span class="nt">"handlers"</span><span class="p">:</span> <span class="p">[</span><span class="s2">"console"</span><span class="p">,</span> <span class="s2">"info_file_handler"</span><span class="p">,</span> <span class="s2">"error_file_handler"</span><span class="p">]</span>
    <span class="p">}</span>
<span class="p">}</span>
</pre>
  </div>
  
  <p>
    logging.yaml
  </p>
  
  <div class="highlight">
    <pre><span class="nn">---</span>
<span class="l-Scalar-Plain">version</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">1</span>
<span class="l-Scalar-Plain">disable_existing_loggers</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">False</span>
<span class="l-Scalar-Plain">formatters</span><span class="p-Indicator">:</span>
    <span class="l-Scalar-Plain">simple</span><span class="p-Indicator">:</span>
        <span class="l-Scalar-Plain">format</span><span class="p-Indicator">:</span> <span class="s">"%(asctime)s</span><span class="s">-</span><span class="s">%(name)s</span><span class="s">-</span><span class="s">%(levelname)s</span><span class="s">-</span><span class="s">%(message)s"</span>

<span class="l-Scalar-Plain">handlers</span><span class="p-Indicator">:</span>
    <span class="l-Scalar-Plain">console</span><span class="p-Indicator">:</span>
        <span class="l-Scalar-Plain">class</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">logging.StreamHandler</span>
        <span class="l-Scalar-Plain">level</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">DEBUG</span>
        <span class="l-Scalar-Plain">formatter</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">simple</span>
        <span class="l-Scalar-Plain">stream</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">ext://sys.stdout</span>

    <span class="l-Scalar-Plain">info_file_handler</span><span class="p-Indicator">:</span>
        <span class="l-Scalar-Plain">class</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">logging.handlers.RotatingFileHandler</span>
        <span class="l-Scalar-Plain">level</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">INFO</span>
        <span class="l-Scalar-Plain">formatter</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">simple</span>
        <span class="l-Scalar-Plain">filename</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">info.log</span>
        <span class="l-Scalar-Plain">maxBytes</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">10485760</span> <span class="c1"># 10MB</span>
        <span class="l-Scalar-Plain">backupCount</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">20</span>
        <span class="l-Scalar-Plain">encoding</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">utf8</span>

    <span class="l-Scalar-Plain">error_file_handler</span><span class="p-Indicator">:</span>
        <span class="l-Scalar-Plain">class</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">logging.handlers.RotatingFileHandler</span>
        <span class="l-Scalar-Plain">level</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">ERROR</span>
        <span class="l-Scalar-Plain">formatter</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">simple</span>
        <span class="l-Scalar-Plain">filename</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">errors.log</span>
        <span class="l-Scalar-Plain">maxBytes</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">10485760</span> <span class="c1"># 10MB</span>
        <span class="l-Scalar-Plain">backupCount</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">20</span>
        <span class="l-Scalar-Plain">encoding</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">utf8</span>

<span class="l-Scalar-Plain">loggers</span><span class="p-Indicator">:</span>
    <span class="l-Scalar-Plain">my_module</span><span class="p-Indicator">:</span>
        <span class="l-Scalar-Plain">level</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">ERROR</span>
        <span class="l-Scalar-Plain">handlers</span><span class="p-Indicator">:</span> <span class="p-Indicator">[</span><span class="nv">console</span><span class="p-Indicator">]</span>
        <span class="l-Scalar-Plain">propagate</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">no</span>

<span class="l-Scalar-Plain">root</span><span class="p-Indicator">:</span>
    <span class="l-Scalar-Plain">level</span><span class="p-Indicator">:</span> <span class="l-Scalar-Plain">INFO</span>
    <span class="l-Scalar-Plain">handlers</span><span class="p-Indicator">:</span> <span class="p-Indicator">[</span><span class="nv">console</span><span class="p-Indicator">,</span> <span class="nv">info_file_handler</span><span class="p-Indicator">,</span> <span class="nv">error_file_handler</span><span class="p-Indicator">]</span>
<span class="nn">...</span>
</pre>
  </div>
  
  <p>
    Following recipe shows you how to read logging configuration from a JSON file:
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">os</span>
<span class="kn">import</span> <span class="nn">json</span>
<span class="kn">import</span> <span class="nn">logging.config</span>

<span class="k">def</span> <span class="nf">setup_logging</span><span class="p">(</span>
    <span class="n">default_path</span><span class="o">=</span><span class="s">'logging.json'</span><span class="p">,</span>
    <span class="n">default_level</span><span class="o">=</span><span class="n">logging</span><span class="o">.</span><span class="n">INFO</span><span class="p">,</span>
    <span class="n">env_key</span><span class="o">=</span><span class="s">'LOG_CFG'</span>
<span class="p">):</span>
    <span class="sd">"""Setup logging configuration</span>

<span class="sd"> """</span>
    <span class="n">path</span> <span class="o">=</span> <span class="n">default_path</span>
    <span class="n">value</span> <span class="o">=</span> <span class="n">os</span><span class="o">.</span><span class="n">getenv</span><span class="p">(</span><span class="n">env_key</span><span class="p">,</span> <span class="bp">None</span><span class="p">)</span>
    <span class="k">if</span> <span class="n">value</span><span class="p">:</span>
        <span class="n">path</span> <span class="o">=</span> <span class="n">value</span>
    <span class="k">if</span> <span class="n">os</span><span class="o">.</span><span class="n">path</span><span class="o">.</span><span class="n">exists</span><span class="p">(</span><span class="n">path</span><span class="p">):</span>
        <span class="k">with</span> <span class="nb">open</span><span class="p">(</span><span class="n">path</span><span class="p">,</span> <span class="s">'rt'</span><span class="p">)</span> <span class="k">as</span> <span class="n">f</span><span class="p">:</span>
            <span class="n">config</span> <span class="o">=</span> <span class="n">json</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="n">f</span><span class="o">.</span><span class="n">read</span><span class="p">())</span>
        <span class="n">logging</span><span class="o">.</span><span class="n">config</span><span class="o">.</span><span class="n">dictConfig</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>
    <span class="k">else</span><span class="p">:</span>
        <span class="n">logging</span><span class="o">.</span><span class="n">basicConfig</span><span class="p">(</span><span class="n">level</span><span class="o">=</span><span class="n">default_level</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    One advantage of using JSON configuration is that the json is a standard library, you don’t need to install it. But personally, I prefer YAML. It’s very clear to read and easy to write. You can also load the YAML configuration with following recipes
  </p>
  
  <div class="highlight">
    <pre><span class="kn">import</span> <span class="nn">os</span>
<span class="kn">import</span> <span class="nn">logging.config</span>

<span class="kn">import</span> <span class="nn">yaml</span>

<span class="k">def</span> <span class="nf">setup_logging</span><span class="p">(</span>
    <span class="n">default_path</span><span class="o">=</span><span class="s">'logging.yaml'</span><span class="p">,</span>
    <span class="n">default_level</span><span class="o">=</span><span class="n">logging</span><span class="o">.</span><span class="n">INFO</span><span class="p">,</span>
    <span class="n">env_key</span><span class="o">=</span><span class="s">'LOG_CFG'</span>
<span class="p">):</span>
    <span class="sd">"""Setup logging configuration</span>

<span class="sd"> """</span>
    <span class="n">path</span> <span class="o">=</span> <span class="n">default_path</span>
    <span class="n">value</span> <span class="o">=</span> <span class="n">os</span><span class="o">.</span><span class="n">getenv</span><span class="p">(</span><span class="n">env_key</span><span class="p">,</span> <span class="bp">None</span><span class="p">)</span>
    <span class="k">if</span> <span class="n">value</span><span class="p">:</span>
        <span class="n">path</span> <span class="o">=</span> <span class="n">value</span>
    <span class="k">if</span> <span class="n">os</span><span class="o">.</span><span class="n">path</span><span class="o">.</span><span class="n">exists</span><span class="p">(</span><span class="n">path</span><span class="p">):</span>
        <span class="k">with</span> <span class="nb">open</span><span class="p">(</span><span class="n">path</span><span class="p">,</span> <span class="s">'rt'</span><span class="p">)</span> <span class="k">as</span> <span class="n">f</span><span class="p">:</span>
            <span class="n">config</span> <span class="o">=</span> <span class="n">yaml</span><span class="o">.</span><span class="n">load</span><span class="p">(</span><span class="n">f</span><span class="o">.</span><span class="n">read</span><span class="p">())</span>
        <span class="n">logging</span><span class="o">.</span><span class="n">config</span><span class="o">.</span><span class="n">dictConfig</span><span class="p">(</span><span class="n">config</span><span class="p">)</span>
    <span class="k">else</span><span class="p">:</span>
        <span class="n">logging</span><span class="o">.</span><span class="n">basicConfig</span><span class="p">(</span><span class="n">level</span><span class="o">=</span><span class="n">default_level</span><span class="p">)</span>
</pre>
  </div>
  
  <p>
    Now, to setup logging, you can call setup_logging when starting your program. It reads logging.json or logging.yaml by default. You can also set LOG_CFG environment variable to load the logging configuration from specific path. For example,
  </p>
  
  <div class="highlight">
    <pre><span class="n">LOG_CFG</span><span class="o">=</span><span class="n">my_logging</span><span class="p">.</span><span class="n">json</span> <span class="n">python</span> <span class="n">my_server</span><span class="p">.</span><span class="n">py</span>
</pre>
  </div>
  
  <p>
    or if you prefer YAML
  </p>
  
  <div class="highlight">
    <pre><span class="n">LOG_CFG</span><span class="o">=</span><span class="n">my_logging</span><span class="p">.</span><span class="n">yaml</span> <span class="n">python</span> <span class="n">my_server</span><span class="p">.</span><span class="n">py</span>
</pre>
  </div>
  
  <h3>
    Use rotating file handler
  </h3>
  
  <p>
    If you use FileHandler for writing logs, the size of log file will grow with time. Someday, it will occupy all of your disk. In order to avoid that situation, you should use RotatingFileHandler instead of FileHandler in production environment.
  </p>
  
  <h3>
    Setup a central log server when you have multiple servers
  </h3>
  
  <p>
    When you have multiple servers and different log files. You can setup a central log system to collect all important (warning and error messages in most cases). Then you can monitor it easily and notice what’s wrong in your system.
  </p>
  
  <h3>
    Conclusions
  </h3>
  
  <p>
    I’m glad that Python logging library is nicely designed, and the best part is that it is a standard library, you don’t have to choose. It is flexible, you can write your own handlers and filters. There are also third-party handlers such as <a href="http://zeromq.org">ZeroMQ</a> logging handler provided by <a href="http://zeromq.github.io/pyzmq/">pyzmq</a>, it allows you to send logging messages through a zmq socket. If you don’t know how to use the logging system correctly, this article might be helpful. With good logging practice, you can find issues in your system easier. It’s a nice investment, don’t you buy it? <img src='http://localhost/wordpress/wp-content/themes/d8/img/smilies/icon_biggrin.gif' alt=':D' class='wp-smiley' />
  </p>
</div>

转载请注明：[于哲的博客][1] &raquo; [【Python】Good logging practice in Python][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3372.html