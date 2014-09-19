---
title: Asp.Net笔记（1）：生成验证码
author: Flowerowl
layout: post
permalink: /2201.html
duoshuo_thread_id:
  - 2454337
views:
  - 606
bot_views:
  - 6
categories:
  - Asp.Net
  - 代码
---
[<img class="alignnone size-full wp-image-2202" title="valiade" src="http://lazynight.me/wp-content/uploads/2012/06/valiade.gif" alt="" width="429" height="61" />][1]

<pre class="lang:default decode:true">using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using System.IO;

public partial class index : System.Web.UI.Page
{
    protected void Page_Load(object sender, EventArgs e)
    {
        if (!IsPostBack)
        {
            //生成4位随机字符串
            string validateNum = CreateRandomNum(4);
            //将随机字符串绘成图片
            CreateImage(validateNum);
            //保存验证码
            Session["ValiadateNum"] = validateNum;
            State.Text=validateNum;
        }

    }
    private string CreateRandomNum(int NumCount)
    {
        string allChar = "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,W,X,Y,Z";
        string[] allCharArray = allChar.Split(',');
        string randomNum = "";
        int temp = -1;
        Random rand = new Random();
        for (int i = 0; i &lt; NumCount; i++)
        {
            if (temp != -1)
            {
                rand = new Random(i * temp * ((int)DateTime.Now.Ticks));
            }
            int t = rand.Next(35);
            if (temp == t)
            {
                return CreateRandomNum(NumCount);
            }
            temp = t;
            randomNum += allCharArray[t];
        }
        return randomNum;
    }
    private void CreateImage(string validateNum)
    {
        if (validateNum == null || validateNum.Trim() == string.Empty)
        {
            return;
        }
        Bitmap image = new Bitmap(validateNum.Length*12+10,22);
        Graphics g = Graphics.FromImage(image);
        try
        {
            Random random = new Random();
            g.Clear(Color.White);
            //背景噪线
            for (int i = 0; i &lt; 25; i++)
            {
                int x1 = random.Next(image.Width);
                int x2 = random.Next(image.Width);
                int y1 = random.Next(image.Height);
                int y2 = random.Next(image.Height);
                g.DrawLine(new Pen(Color.Silver),x1,y1,x2,y2);
            }
            Font font = new Font("Arial",12,FontStyle.Bold|FontStyle.Italic);
            LinearGradientBrush brush = new LinearGradientBrush(new Rectangle(0,0,image.Width,image.Height),Color.Blue,Color.DarkRed,1.2f,true);
            g.DrawString(validateNum,font,brush,2,2);
            //前景噪点
            for (int i = 0; i &lt; 100; i++)
            {
                int x = random.Next(image.Width);
                int y = random.Next(image.Height);
                image.SetPixel(x,y,Color.FromArgb(random.Next()));
            }
            //图片边框线
            g.DrawRectangle(new Pen(Color.Silver),0,0,image.Width-1,image.Height-1);
            MemoryStream ms = new MemoryStream();
            image.Save(ms,ImageFormat.Gif);
            Response.ClearContent();
            Response.ContentType = "image/gif";
            Response.BinaryWrite(ms.ToArray());
        }
        finally
        {
            g.Dispose();
            image.Dispose();
        }

    }
}</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [Asp.Net笔记（1）：生成验证码][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/06/valiade.gif
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2201.html