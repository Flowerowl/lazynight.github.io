---
title: PhantomJS 学习笔记（2）：生成网页截图
author: Flowerowl
layout: post
permalink: /2714.html
views:
  - 2209
duoshuo_thread_id:
  - 1220743779864322419
bot_views:
  - 1
categories:
  - PhantomJS
  - 代码
tags:
  - PhantomJS
---
网上有使用C#调用phantomjs来批量生成网站截图的，可以来供界面设计师参考~

核心的截取代码如下：其实也就是官方的一个demo~

不过有时候对中文不够支持，比如截取baidu，会产生乱码。

<pre class="lang:default decode:true">var page = require('webpage').create(),
    system = require('system'),
    address, output, size;

if (system.args.length &lt; 3 || system.args.length &gt; 5) {
    console.log('Usage: rasterize.js URL filename [paperwidth*paperheight|paperformat] [zoom]');
    console.log('  paper (pdf output) examples: "5in*7.5in", "10cm*20cm", "A4", "Letter"');
    phantom.exit(1);
} else {
    address = system.args[1];
    output = system.args[2];
    page.viewportSize = { width: 600, height: 600 };
    if (system.args.length &gt; 3 && system.args[2].substr(-4) === ".pdf") {
        size = system.args[3].split('*');
        page.paperSize = size.length === 2 ? { width: size[0], height: size[1], margin: '0px' }
                                           : { format: system.args[3], orientation: 'portrait', margin: '1cm' };
    }
    if (system.args.length &gt; 4) {
        page.zoomFactor = system.args[4];
    }
    page.open(address, function (status) {
        if (status !== 'success') {
            console.log('Unable to load the address!');
        } else {
            window.setTimeout(function () {
                page.render(output);
                phantom.exit();
            }, 200);
        }
    });
}</pre>

> 使用命令：matoMacBook-Pro:examples Flowerowl$ phantomjs rasterize.js http://baidu.com  logo.gif

运行截图：

[<img class="alignnone size-full wp-image-2715" title="logo" src="http://lazynight.me/wp-content/uploads/2012/11/logo.gif" alt="" width="720" height="600" />][1]

C#调用版：

<pre class="lang:default decode:true">using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;
using System.IO;
using System.Runtime.Serialization.Formatters.Binary;
using System.Threading;
using System.Diagnostics;

namespace WebScreen
{
    public partial class Main : Form
    {
        public Main()
        {
            InitializeComponent();
        }

        public static  List&lt;string&gt; list_Url = new List&lt;string&gt;();

        private void Main_Load(object sender, EventArgs e)
        {
            loadList();

        }

        List&lt;string&gt; list_url_temp = new List&lt;string&gt;();
        private void button_Start_Click(object sender, EventArgs e)
        {

            Thread th = new Thread(_StartTest);
            th.IsBackground = false;
            th.Start();

        }

        void _StartTest() {

            if (list_Url.Count &gt; 0)
            {
                this.BeginInvoke((MethodInvoker)delegate { list_Msg.Items.Add("开始进行任务"); });
                foreach (string url in list_Url)
                {
                    this.BeginInvoke((MethodInvoker)delegate { list_Msg.Items.Add("正在进行[" + url + "]站点截图"); });
                    DateTime beginTime = DateTime.Now;
                    string Image = url;
                    GetImage(url);
                    int ii = 0;
                    while (true)
                    {
                        if (File.Exists("pic/" + url + ".png"))
                        {
                            break;
                        }
                        else
                        {
                            if (ii &gt; 60)
                            {
                                break;
                            }
                            Thread.Sleep(1000);
                            ii++;
                        }

                    }
                    DateTime endTime = DateTime.Now;
                    TimeSpan ts = endTime.Subtract(beginTime);
                    this.BeginInvoke((MethodInvoker)delegate { list_Msg.Items.Add("[" + Image + "]站点截图已保存,耗时：" + ts.TotalMilliseconds + "毫秒"); });
                }
                this.BeginInvoke((MethodInvoker)delegate { list_Msg.Items.Add("所有任务已经完成"); });
                Process.Start("explorer.exe", Environment.CurrentDirectory + "\\pic\\");
            }
            else
            {
                MessageBox.Show("列表无数据");
            }

        }

        private void GetImage(string url) {

            string links = url.IndexOf("http://") &gt; -1 ? url : "http://" + url;

            #region 启动进程
            Process p = new Process();
            p.StartInfo.FileName = Environment.CurrentDirectory+"//phantomjs.exe";
            p.StartInfo.WorkingDirectory = Environment.CurrentDirectory+"//pic//";
            p.StartInfo.Arguments = string.Format("--ignore-ssl-errors=yes --load-plugins=yes " + Environment.CurrentDirectory + "//rasterize.js  " + links + " "+url+".png");

            p.StartInfo.CreateNoWindow = true;
            p.StartInfo.WindowStyle = ProcessWindowStyle.Hidden;

            if (!p.Start())
                throw new Exception("无法Headless浏览器.");

            #endregion

        }

        private void button_List_Click(object sender, EventArgs e)
        {
            UrlList form_UrlList = new UrlList(list_Url);
            form_UrlList.ShowDialog();
            list_Url = form_UrlList.list;
            this.Text = "检测网页数量：" + list_Url.Count;
        }

        private static void savelist()
        {
            lock (list_Url)
            {
                using (FileStream fs = new FileStream("url.list", FileMode.Create))
                {
                    if (list_Url != null)
                    {
                        BinaryFormatter formatter = new BinaryFormatter();
                        formatter.Serialize(fs, list_Url);
                    }
                }
            }
        }

        private static void loadList()
        {
            lock (list_Url)
            {
                list_Url.Clear();
                if (File.Exists("url.list"))
                {
                    using (FileStream fs = new FileStream("url.list", FileMode.OpenOrCreate))
                    {
                        BinaryFormatter formatter = new BinaryFormatter();
                        list_Url = (List&lt;string&gt;)formatter.Deserialize(fs);
                    }
                }
            }
        }

        private void Main_FormClosing(object sender, FormClosingEventArgs e)
        {
            savelist();
        }

    }
}</pre>

源码参考：http://www.cnblogs.com/yesicoo/archive/2012/05/25/2518729.html

运行截图：

[<img class="alignnone size-full wp-image-2716" title="c#phantom" src="http://lazynight.me/wp-content/uploads/2012/11/cphantom.jpg" alt="" width="650" height="439" />][2]

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [PhantomJS 学习笔记（2）：生成网页截图][4]

 [1]: http://lazynight.me/wp-content/uploads/2012/11/logo.gif
 [2]: http://lazynight.me/wp-content/uploads/2012/11/cphantom.jpg
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/2714.html