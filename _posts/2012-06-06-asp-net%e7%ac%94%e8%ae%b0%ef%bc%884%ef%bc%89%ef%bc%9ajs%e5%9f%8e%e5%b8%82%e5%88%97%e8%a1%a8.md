---
title: Asp.Net笔记（4）：JS城市列表
author: Flowerowl
layout: post
permalink: /2254.html
duoshuo_thread_id:
  - 2638002
views:
  - 714
bot_views:
  - 6
categories:
  - Asp.Net
  - 代码
---
[<img class="alignnone size-full wp-image-2255" title="4" src="http://lazynight.me/wp-content/uploads/2012/06/4.gif" alt="" width="181" height="91" />][1]

<pre class="lang:default decode:true">&lt;%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %&gt;

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;

&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head runat="server"&gt;
    &lt;title&gt;&lt;/title&gt;
    &lt;script type="text/javascript" language="javascript"&gt;
        //定义存储省市信息的JSON集合
        var cityList = [
            { "provider": "广西", "citys": ["南宁", "柳州", "桂林"] },
            { "provider": "四川", "citys": ["成都", "绵阳", "德阳"] },
            { "provider": "新疆", "citys": ["乌鲁木齐", "喀什", "库尔勒"] }
        ];
        function changeCity() {
            //清空城市下拉列表
            document.getElementById("slcCitys").options.length = 0;
            //获取选中的省份选项
            var option = document.getElementById("slcProvider").options[document.getElementById("slcProvider").selectedIndex];
            //循环遍历JSON集合
            for (var i = 0; i &lt; cityList.length; i++) {
                //判断JSON省份项与当前选中的省份是否一致
                if (cityList[i].provider == option.innerText) {
                    //从JSON集合中取出城市列表
                    for (var j = 0; j &lt; cityList[i].citys.length; j++) {
                        var o = document.createElement("option");
                        o.innerText = cityList[i].citys[j];
                        document.getElementById("slcCitys").appendChild(o);
                    }
                }
            }
        }
    &lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;form id="form1" runat="server"&gt;
    &lt;div&gt;
        &lt;select runat="server" id="slcProvider" onchange="changeCity()"&gt;
            &lt;option selected="true"&gt;请选择城市&lt;/option&gt;
            &lt;option&gt;广西&lt;/option&gt;
            &lt;option&gt;四川&lt;/option&gt;
            &lt;option&gt;新疆&lt;/option&gt;
        &lt;/select&gt;
        &lt;select runat="server" id="slcCitys"&gt;
            &lt;option selected="true"&gt;请选择城市&lt;/option&gt;
        &lt;/select&gt;
    &lt;/div&gt;
    &lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [Asp.Net笔记（4）：JS城市列表][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/4.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2254.html