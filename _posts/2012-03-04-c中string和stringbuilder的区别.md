---
title: 'C#中string和stringBuilder的区别'
author: Flowerowl
layout: post
permalink: /1406.html
duoshuo_thread_id:
  - 1266969
views:
  - 713
bot_views:
  - 3
categories:
  - 'C#'
  - 技术杂谈
---
<span style="color: #ff6600;">看完这个，我觉得之前写的“夜阑MP3标签修改器”之所以占内存大的缘故就明了了，至少这是一方面。</span>

&nbsp;

* * *

String对象是不可改变的。每次使用System.String类中的方法之一或者是进行运算时（如赋值、拼接等），都要在内存中创建一个新的字符串对象，这就需要为该新对象分配内存空间，而StringBuilder则不会。在需要对字符串执行重复修改操作时，与创建新的 String 对象相关的系统开销可能会非常昂贵。如果要修改字符串而不创建新的对象，则可以使用 System.Text.StringBuilder 类。例如，当在一个循环中将许多字符串连接在一起时，使用 StringBuilder 类可以提升性能。

String类型对象的特点：

1.它是引用类型，在堆上分配内存

2.运算时会产生一个新的实例

3.String 对象一旦生成不可改变（Immutable）

4.定义相等运算符（**==** 和 **!=**）是为了比较 String 对象的值（而不是引用）

大家都知道字符串对象是“不可变的”,  
对字符串进行操作的方法实际上返回的是新的字符串对象。  
在前面的示例中，将 `s1` 和 `s2` 的内容连接起来以构成一个字符串时，包含 `"orange"` 和 `"red"` 的两个字符串均保持不变。**+=** 运算符会创建一个包含组合内容的新字符串。结果是 `s1` 现在引用一个完全不同的字符串。只包含`"orange" `的字符串仍然存在，但连接 `s1` 后将不再被引用。  
大量的字符串相加的时候就会有很多想s1一样的 不在被引用,从而造成资源的极大浪费.

大家注意这点

string stringValue = this.m_StringValue;

internal volatile string m_StringValue;

写到这里,需要有人见看到了 volatile,也许不明白是什么意思,大概的说下.

volatile关键字实现了线程间数据同步,用volatile修饰后的变量不允许有不同于“主”内存区域的变量拷贝。

换句话说，一个变量经volatile修饰后在所有线程中必须是同步的;任何线程中改变了它的值，所有其他线程立即

获取到了相同的值。理所当然的，volatile修饰的变量存取时比一般变量消耗的资源要多一点，因为线程有它自己的

变量拷贝更为高效。

this.NeedsAllocation(stringValue, requiredLength)

只有在需要的时候才去重新分配.

就分配空间和线程的使用上来讲,StringBuilder肯定比String要高,但是前提是使用频率比较高的情况下.  
====================================================================  
使用    StringBuilder  
String  对象是不可改变的。每次使用    System.String    类中的方法之一时，都要在内存中创建一个新的字符串对象，这就需要为该新对象分配新的空间。在需要对字符串执行重复修改的情况下，与创建新的    String    对象相关的系统开销可能会非常昂贵。如果要修改字符串而不创建新的对象，则可以使用 System.Text.StringBuilder 类。  
例如，当在一个循环中将许多字符串连接在一起时，使用    StringBuilder    类可以提升性能。

通过用一个重载的构造函数方法初始化变量，可以创建    StringBuilder    类的新实例，如下例

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;);

设置容量和长度  
虽然    StringBuilder    对象是动态对象，允许扩充它所封装的字符串中字符的数量，但是您可以为它可容纳的最大字符数指定一个值。此值称为该对象的容量，不应将它与当前    StringBuilder    对象容纳的字符串长度混淆在一起。例如，可以创建    StringBuilder    类的带有字符串“Hello”（长度为    5）的一个新实例，同时可以指定该对象的最大容量为    25。当修改    StringBuilder    时，在达到容量之前，它不会为其自己重新分配空间。当达到容量时，将自动分配新的空间且容量翻倍。可以使用重载的构造函数之一来指定    StringBuilder    类的容量。以下代码示例指定可以将    MyStringBuilder    对象扩充到最大    25    个空白。

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;,    25);

另外，可以使用读/写    Capacity    属性来设置对象的最大长度。以下代码示例使用    Capacity    属性来定义对象的最大长度。

[C#]  
MyStringBuilder.Capacity    =    25;

EnsureCapacity    方法可用来检查当前    StringBuilder    的容量。如果容量大于传递的值，则不进行任何更改；但是，如果容量小于传递的值，则会更改当前的容量以使其与传递的值匹配。

也可以查看或设置    Length    属性。如果将    Length    属性设置为大于    Capacity    属性的值，则自动将    Capacity    属性更改为与    Length    属性相同的值。如果将    Length    属性设置为小于当前    StringBuilder    对象内的字符串长度的值，则会缩短该字符串。

修改    StringBuilder    字符串  
下表列出了可以用来修改    StringBuilder    的内容的方法。

方法名    使用  
StringBuilder.Append    将信息追加到当前    StringBuilder    的结尾。  
StringBuilder.AppendFormat    用带格式文本替换字符串中传递的格式说明符。  
StringBuilder.Insert    将字符串或对象插入到当前    StringBuilder    对象的指定索引处。  
StringBuilder.Remove    从当前    StringBuilder    对象中移除指定数量的字符。  
StringBuilder.Replace    替换指定索引处的指定字符。

Append  
Append    方法可用来将文本或对象的字符串表示形式添加到由当前    StringBuilder    对象表示的字符串的结尾处。以下示例将一个    StringBuilder    对象初始化为“Hello    World”，然后将一些文本追加到该对象的结尾处。将根据需要自动分配空间。

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;);  
MyStringBuilder.Append(&#8220;    What    a    beautiful    day.&#8221;);  
Console.WriteLine(MyStringBuilder);

此示例将    Hello    World!    What    a    beautiful    day.    显示到控制台。

AppendFormat  
AppendFormat    方法将文本添加到    StringBuilder    的结尾处，而且实现了    IFormattable    接口，因此可接受格式化部分中描述的标准格式字符串。可以使用此方法来自定义变量的格式并将这些值追加到    StringBuilder    的后面。以下示例使用    AppendFormat    方法将一个设置为货币值格式的整数值放置到    StringBuilder    的结尾。

[C#]  
int    MyInt    =    25;  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Your    total    is    &#8220;);  
MyStringBuilder.AppendFormat(&#8220;{0:C}    &#8220;,    MyInt);  
Console.WriteLine(MyStringBuilder);

此示例将    Your    total    is    $25.00    显示到控制台。

Insert  
Insert    方法将字符串或对象添加到当前    StringBuilder    中的指定位置。以下示例使用此方法将一个单词插入到    StringBuilder    的第六个位置。

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;);  
MyStringBuilder.Insert(6,&#8221;Beautiful    &#8220;);  
Console.WriteLine(MyStringBuilder);

此示例将    Hello    Beautiful    World!    显示到控制台。

Remove  
可以使用    Remove    方法从当前    StringBuilder    中移除指定数量的字符，移除过程从指定的从零开始的索引处开始。以下示例使用    Remove    方法缩短    StringBuilder。

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;);  
MyStringBuilder.Remove(5,7);  
Console.WriteLine(MyStringBuilder);

此示例将    Hello    显示到控制台。

Replace  
使用    Replace    方法，可以用另一个指定的字符来替换    StringBuilder    对象内的字符。以下示例使用    Replace    方法来搜索    StringBuilder    对象，查找所有的感叹号字符    (!)，并用问号字符    (?)    来替换它们。

[C#]  
StringBuilder    MyStringBuilder    =    new    StringBuilder(&#8220;Hello    World!&#8221;);  
MyStringBuilder.Replace(&#8216;!&#8217;,    &#8216;?&#8217;);  
Console.WriteLine(MyStringBuilder);

此示例将    Hello    World?    显示到控制台。

转自：<span style="color: #ff6600;"><a href="http://blog.csdn.net/yuzhoufeng888/article/details/6057260"><span style="color: #ff6600;">http://blog.csdn.net/yuzhoufeng888/article/details/6057260</span></a></span>

转载请注明：[于哲的博客][1] &raquo; [C#中string和stringBuilder的区别][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/1406.html