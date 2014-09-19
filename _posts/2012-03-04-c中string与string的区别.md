---
title: 'C#中String与string的区别'
author: Flowerowl
layout: post
permalink: /1400.html
duoshuo_thread_id:
  - 1266968
views:
  - 893
bot_views:
  - 3
categories:
  - 'C#'
  - 技术杂谈
---
<div>
  <p>
    我一直纳闷C#中的string和String的区别，今天找了点资料，贴过来。
  </p>
  
  <p>
    ————————————————————————————————————
  </p>
  
  <p>
    <span style="color: #ff6600;">版本一：</span>
  </p>
  
  <p>
    C#中，字符串的声明，你使用String还是string?
  </p>
  
  <p>
    String? string?
  </p>
  
  <p>
    只有一个大小写的问题，你习惯用哪个？知道具体的区别吗？
  </p>
  
  <p>
    我是习惯了用string，区别也是最近才找到了权威的根据，&#8217;CLR via C#&#8217;。
  </p>
  
  <p>
    其实，String是CLR的类型名称（也算是keyword），而string是C#中的keyword。
  </p>
  
  <p>
    在C#的编译时，默认会增加几行代码，看了你就会明白string和String的区别了！
  </p>
  
  <p>
    using string = System.String; using sbyte = System.SByte;
  </p>
  
  <p>
    using byte = System.Byte; using short = System.Int16;
  </p>
  
  <p>
    using ushort = System.UInt16; using int = System.Int32;
  </p>
  
  <p>
    using uint = System.UInt32; &#8230; &#8230;
  </p>
  
  <p>
    对了！ using string = System.String; C#编译器，会自动的把string转化为Sysem.String!
  </p>
  
  <p>
    在CLR via C#中，Jeffrey Richter建议coding时，使用CLR默认的类型，也就是说，不要string，要String；
  </p>
  
  <p>
    不要int要Int32！至于为什么，还是大家自己看看这本书吧！
  </p>
</div>

<div>
  <ul>
    <li>
      string是c#中的类，String是.net Framework的类(在c# IDE中不会显示蓝色)
    </li>
    <li>
      c# string映射为.net Framework的String
    </li>
    <li>
      如果用string,编译器会把它编译成String，所以如果直接用String就可以让编译器少做一点点工作
    </li>
    <li>
      如果使用c#，建议使用string，比较符合规范
    </li>
    <li>
      string始终代表 System.String(1.x) 或 ::System.String(2.0) ，String只有在前面有using System;的时候并且当前命名空间中没有名为String的类型（class、struct、delegate、enum）的时候才代表System.String
    </li>
    <li>
      string是关键字，String不是，也就是说string不能作为类、结构、枚举、字段、变量、方法、属性的名称，而String可以.
    </li>
  </ul>
  
  <p>
    &nbsp;
  </p>
</div>

————————————————————————————————————

<span style="color: #ff6600;">版本二：</span>

从位置讲：

1.String是.NET   Framework里面的String，小写的string是C#语言中的string

2.如果把using System;删掉，没有大写的String了，System是.NET   Framework类库中的一个函数名.

从性质讲：

1. string是关键字，String是类，string不能作为类、结构、枚举、字段、变量、方法、属性的名称

2. 用C#编写代码的情况下尽量使用小写的string，比较符合规范，如果在追求效率的情况下可以使用大写的String，因为最终通过编译后，小写的string会变成大写的String，可以给编译减少负荷，从而运行效率提高。

3. string 类型表示 Unicode 字符的字符串，string 是 .NET Framework 中的 String 的别名，对字符串相等性的测试更为直观

转载请注明：[于哲的博客][1] &raquo; [C#中String与string的区别][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1400.html