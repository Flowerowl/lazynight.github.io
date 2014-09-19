---
title: WCF笔记2：创建客户端调用服务
author: Flowerowl
layout: post
permalink: /2327.html
duoshuo_thread_id:
  - 3380501
views:
  - 1203
bot_views:
  - 2
categories:
  - WCF
  - 代码
---
<span style="color: #ff0000;">1.</span>通过为Client添加服务引用，右键点击Client项目，在弹出的上下文中选择“添加服务引用”，在地址栏中填写服务元数据的发布源地址

[<img class="alignnone size-full wp-image-2331" title="servi" src="http://lazynight.me/wp-content/uploads/2012/06/servi.gif" alt="" width="635" height="509" />][1]

VS会自动生成一系列的用于服务调用的代码和配置。

在一系列自动生成的类中，包含一个服务契约借口，一个服务代理对象和其他相关的类。

被客户端直接用于服务调用的是一个继承自ClientBase〈CalculatorService〉并实现了CalculatorService接口的服务代理类。

[<img class="alignnone size-full wp-image-2332" title="ser2" src="http://lazynight.me/wp-content/uploads/2012/06/ser2.gif" alt="" width="211" height="375" />][2]

app.config

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;configuration&gt;
    &lt;system.serviceModel&gt;
        &lt;bindings&gt;
            &lt;wsHttpBinding&gt;
                &lt;binding name="WSHttpBinding_CalculatorService" /&gt;
            &lt;/wsHttpBinding&gt;
        &lt;/bindings&gt;
        &lt;client&gt;
            &lt;endpoint address="http://127.0.0.1:9999/calculatorservice" binding="wsHttpBinding"
                bindingConfiguration="WSHttpBinding_CalculatorService" contract="ServiceReference1.CalculatorService"
                name="WSHttpBinding_CalculatorService"&gt;
                &lt;identity&gt;
                    &lt;userPrincipalName value="Flowerowel-PC\Flowerowel" /&gt;
                &lt;/identity&gt;
            &lt;/endpoint&gt;
        &lt;/client&gt;
    &lt;/system.serviceModel&gt;
&lt;/configuration&gt;</pre>

ClientBase〈CalculatorService〉的定义如下所示：

<pre class="lang:default decode:true">//------------------------------------------------------------------------------
// &lt;auto-generated&gt;
//     此代码由工具生成。
//     运行时版本:4.0.30319.17626
//
//     对此文件的更改可能会导致不正确的行为，并且如果
//     重新生成代码，这些更改将会丢失。
// &lt;/auto-generated&gt;
//------------------------------------------------------------------------------

namespace Client.ServiceReference1 {

    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]
    [System.ServiceModel.ServiceContractAttribute(Namespace="Lazynight", ConfigurationName="ServiceReference1.CalculatorService")]
    public interface CalculatorService {

        [System.ServiceModel.OperationContractAttribute(Action="Lazynight/CalculatorService/Add", ReplyAction="Lazynight/CalculatorService/AddResponse")]
        double Add(double x, double y);

        [System.ServiceModel.OperationContractAttribute(Action="Lazynight/CalculatorService/Subtract", ReplyAction="Lazynight/CalculatorService/SubtractResponse")]
        double Subtract(double x, double y);

        [System.ServiceModel.OperationContractAttribute(Action="Lazynight/CalculatorService/Multiply", ReplyAction="Lazynight/CalculatorService/MultiplyResponse")]
        double Multiply(double x, double y);

        [System.ServiceModel.OperationContractAttribute(Action="Lazynight/CalculatorService/Divide", ReplyAction="Lazynight/CalculatorService/DivideResponse")]
        double Divide(double x, double y);
    }

    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]
    public interface CalculatorServiceChannel : Client.ServiceReference1.CalculatorService, System.ServiceModel.IClientChannel {
    }

    [System.Diagnostics.DebuggerStepThroughAttribute()]
    [System.CodeDom.Compiler.GeneratedCodeAttribute("System.ServiceModel", "4.0.0.0")]
    public partial class CalculatorServiceClient : System.ServiceModel.ClientBase&lt;Client.ServiceReference1.CalculatorService&gt;, Client.ServiceReference1.CalculatorService {

        public CalculatorServiceClient() {
        }

        public CalculatorServiceClient(string endpointConfigurationName) : 
                base(endpointConfigurationName) {
        }

        public CalculatorServiceClient(string endpointConfigurationName, string remoteAddress) : 
                base(endpointConfigurationName, remoteAddress) {
        }

        public CalculatorServiceClient(string endpointConfigurationName, System.ServiceModel.EndpointAddress remoteAddress) : 
                base(endpointConfigurationName, remoteAddress) {
        }

        public CalculatorServiceClient(System.ServiceModel.Channels.Binding binding, System.ServiceModel.EndpointAddress remoteAddress) : 
                base(binding, remoteAddress) {
        }

        public double Add(double x, double y) {
            return base.Channel.Add(x, y);
        }

        public double Subtract(double x, double y) {
            return base.Channel.Subtract(x, y);
        }

        public double Multiply(double x, double y) {
            return base.Channel.Multiply(x, y);
        }

        public double Divide(double x, double y) {
            return base.Channel.Divide(x, y);
        }
    }
}</pre>

接下来就可以创建CalculatorServiceClient对象，执行相应的方法调用服务操作，客户端进行服务调用的代码入下：

<pre class="lang:default decode:true">using (CalculatorServiceClient proxy = new CalculatorServiceClient())
            {
                System.Console.WriteLine("x+y={2}when x={0} and y={1}",1,2,proxy.Add(1,2));
            }</pre>

<span style="color: #ff0000;"> 2. </span>通过ChannelFactory<T>。

在上面的例子可以看到客户端会创建一个与服务端等效的服务契约接口。我们可以让服务端和客户端引用相同的契约。

现在讲刚才添加的服务引用删除，并为Client项目添加对Contracts项目的引用，借助于这个服务契约，并通过ChannelFactory<ICalculator>创建服务代理对象，直接进行相应的服务调用。

下面的代码掩饰了基于ChannelFactory<T>进行服务代理的创建和服务调用的方式。

<pre class="lang:default decode:true ">using System;
using System.ServiceModel;
using Contracts;

namespace Client
{
    class Program
    {
        static void Main(string[] args)
        {
            using (ChannelFactory&lt;ICaculator&gt; ChannelFactory = new ChannelFactory&lt;ICaculator&gt;(new WSHttpBinding(), "http://127.0.0.1:9999/calculatorservice"))
            {
                ICaculator proxy = ChannelFactory.CreateChannel();
                using (proxy as IDisposable)
                {
                    Console.WriteLine("x+y={2} when x={0} and y={1}",1,2,proxy.Add(1,2));
                    Console.WriteLine("x-y={2} when x={0} and y={1}",2,1,proxy.Subtract(2,1));
                }
            }
        }
    }
}</pre>

<span style="color: #ff0000;">3.</span>简化第二种方法

由于终结点是WCF进行通信的唯一手段，ChannelFactory<T>本质上是通过制定的终结点创建用于进行服务调用的服务代理。

在上面的代码中，在创建ChannelFactory<T>的时候再在构造函数中制定终结点的相关要素（契约通过泛型类表示，地址和绑定则通过参数制定）。

在真正的WCF应用中，大都采用配置的方式进行终结点的定义。我们可以通过下面的配置制定终结点的三要素，并为相应的终结点制定一个终结点配置名称（calculatorservice）。

那么在创建ChannelFactory<T>的时候，就无需再制定终结点的绑定和地址了，只需制定对应的终结点配置名称。

<pre class="lang:default decode:true">&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;configuration&gt;
    &lt;system.serviceModel&gt;
      &lt;client&gt;
        &lt;endpoint address="http://127.0.0.1:9999/calculatorservice" binding="wsHttpBinding" contract="Contracts.ICaculator" name="calculatorservice"/&gt;
      &lt;/client&gt;
    &lt;/system.serviceModel&gt;
&lt;/configuration&gt;</pre>

&nbsp;

<pre class="lang:default decode:true">using System;
using System.ServiceModel;
using Contracts;

namespace Client
{
    class Program
    {
        static void Main(string[] args)
        {
            using (ChannelFactory&lt;ICaculator&gt; channelfactory = new ChannelFactory&lt;ICaculator&gt;("calculatorservice"))
            {
                ICaculator proxy=channelfactory.CreateChannel();
                using (proxy as IDisposable)
                {
                    Console.WriteLine("x+y={2} when x={0} and y={1}",1,2,proxy.Add(1,2));
                }

            }
        }
    }
}</pre>

&nbsp;

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [WCF笔记2：创建客户端调用服务][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/servi.gif
 [2]: http://lazynight.me/wp-content/uploads/2012/06/ser2.gif
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2327.html