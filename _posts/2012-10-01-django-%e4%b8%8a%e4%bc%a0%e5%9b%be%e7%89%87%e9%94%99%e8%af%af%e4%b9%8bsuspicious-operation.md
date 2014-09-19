---
title: 'Django 上传图片错误之&#8221;suspicious operation&#8221;'
author: Flowerowl
layout: post
permalink: /2534.html
duoshuo_thread_id:
  - 9469978
views:
  - 965
bot_views:
  - 2
categories:
  - Django
  - 技术杂谈
---
错误：安装Python Image Library之后，Django里上传图片出错：suspicious operation

解决：Models.py里的存储路径应该和settings.py里的MEDIA_ROOT保持一致。

我们 的Models.py内容为

<pre class="lang:default decode:true">from django.db import models
from django.contrib import admin

class Publisher(models.Model):
    name = models.CharField(max_length=30)
    address = models.CharField(max_length=50)
    city = models.CharField(max_length=60)
    state_province = models.CharField(max_length=30)
    country = models.CharField(max_length=50)
    website = models.URLField()

class Author(models.Model):
    salutation = models.CharField(max_length=10)
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=40)
    email = models.EmailField()
    headshot = models.ImageField(upload_to='/z/tmp')

class Book(models.Model):
    title = models.CharField(max_length=100)
    authors = models.ManyToManyField(Author)
    publisher = models.ForeignKey(Publisher)
    publication_date = models.DateField()

admin.site.register(Publisher)
admin.site.register(Author)
admin.site.register(Book)</pre>

这里

<pre>headshot = models.ImageField(upload_to='/z/tmp')</pre>

路径应该与settings.py 的MEDIA_ROOT一致：

<pre class="lang:default decode:true ">MEDIA_ROOT = '/z/tmp'</pre>

&nbsp;

转载请注明：[于哲的博客][1] &raquo; [Django 上传图片错误之&#8221;suspicious operation&#8221;][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2534.html