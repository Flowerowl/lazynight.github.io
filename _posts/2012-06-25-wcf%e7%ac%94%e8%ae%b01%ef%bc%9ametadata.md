---
title: WCF笔记1：metadata
author: Flowerowl
layout: post
permalink: /2321.html
duoshuo_thread_id:
  - 3367320
views:
  - 833
bot_views:
  - 3
categories:
  - WCF
  - 代码
---
[<img class="alignnone size-full wp-image-2322" title="project" src="http://lazynight.me/wp-content/uploads/2012/06/project.gif" alt="" width="198" height="334" />][1]

采用wsHttpBinding绑定。

<div>
  下面的代码显示了wsHttpBinding绑定的地址格式：
</div>

<div>
  http://{hostname}:{port}/{service location}
</div>

<div>
  https://{hostname}:{port}/{service location}
</div>

<div>
  http协议的默认端口是80而https的默认端口是443。
</div>

<div>
</div>

<div>
  实现metadata暴露的方法有二：
</div>

<div>
  1.在Hosting代码中编写：
</div>

<div>
  <pre class="lang:default decode:true ">using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Contracts;
using Services;

namespace Hosting
{
    class Program
    {
        static void Main(string[] args)
        {
            using (ServiceHost host = new ServiceHost(typeof(Services.Services)))
            {
                host.AddServiceEndpoint(typeof(ICaculator), new WSHttpBinding(), "http://127.0.0.1:9999/calculatorservice");
                if (host.Description.Behaviors.Find&lt;ServiceMetadataBehavior&gt;() == null)
                {
                    ServiceMetadataBehavior behavior = new ServiceMetadataBehavior();
                    behavior.HttpGetEnabled = true;
                    behavior.HttpGetUrl = new Uri("http://127.0.0.1:9999/calculatorservice/metadata");
                    host.Description.Behaviors.Add(behavior);
                }
                host.Opened += delegate { Console.WriteLine("CalculatorService已经启动，按任意键终止服务"); };
                host.Open();
                Console.Read();
            }
        }
    }
}</pre>
  
  <p>
    2.在App.config文件中进行配置
  </p>
  
  <p>
    此时可以把Hosting改写为：
  </p>
  
  <pre class="lang:default decode:true ">using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Contracts;
using Services;

namespace Hosting
{
    class Program
    {
        static void Main(string[] args)
        {
            using (ServiceHost host = new ServiceHost(typeof(Services.Services)))
            {

                host.Opened += delegate { Console.WriteLine("CalculatorService已经启动，按任意键终止服务"); };
                host.Open();
                Console.Read();
            }
        }
    }
}</pre>
  
  <p>
    App.config
  </p>
  
  <pre class="lang:default decode:true ">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
  &lt;configuration&gt;
    &lt;system.serviceModel&gt;
      &lt;behaviors&gt;
        &lt;serviceBehaviors&gt;
          &lt;behavior name="metadataBehavior"&gt;
            &lt;serviceMetadata httpGetEnabled="true" httpGetUrl="http://127.0.0.1:9999/calculatorservice/metadata"/&gt;
          &lt;/behavior&gt;
        &lt;/serviceBehaviors&gt;
      &lt;/behaviors&gt;
      &lt;services&gt;
        &lt;service behaviorConfiguration="metadataBehavior" name="Services.Services"&gt;
          &lt;endpoint address="http://127.0.0.1:9999/calculatorservice" binding="wsHttpBinding" contract="Contracts.ICaculator"/&gt;
        &lt;/service&gt;
      &lt;/services&gt;
    &lt;/system.serviceModel&gt;
  &lt;/configuration&gt;</pre>
  
  <p>
    <a href="http://lazynight.me/wp-content/uploads/2012/06/metadata.gif"><img class="alignnone size-full wp-image-2323" title="metadata" src="http://lazynight.me/wp-content/uploads/2012/06/metadata.gif" alt="" width="536" height="292" /></a>
  </p>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    服务被成功寄宿后，服务端便开始了服务调用请求的监听工作。此外，服务寄宿将服务描述通过元数据的形式发布出来，相应的客户端就可以获取这些元数据，创建客户端程序进行服务的消费。在VS下，当我们添加服务引用的时候，VS在内部帮我们实现元数据的获取，并借助这些元数据通过代码生成工具（svcutil.exe）自动生成用于服务调用的服务代理相关代码和相应的配置。
  </p>
  
  <p>
    <span style="color: #ff0000;"><a href="http://dl.dbank.com/c0tnvq4hky"><span style="color: #ff0000;">下载源码</span></a></span>
  </p>
</div>

转载请注明：[于哲的博客][2] &raquo; [WCF笔记1：metadata][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/project.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2321.html