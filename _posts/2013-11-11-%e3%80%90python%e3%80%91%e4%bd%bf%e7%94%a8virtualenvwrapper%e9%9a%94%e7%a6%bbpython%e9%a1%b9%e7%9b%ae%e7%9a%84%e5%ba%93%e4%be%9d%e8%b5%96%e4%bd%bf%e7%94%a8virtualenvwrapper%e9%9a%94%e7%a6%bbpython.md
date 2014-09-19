---
title: 【Python】使用VirtualEnvWrapper隔离python项目的库依赖
author: Flowerowl
layout: post
permalink: /3194.html
duoshuo_thread_id:
  - 1220743779864322526
views:
  - 1649
bot_views:
  - 2
categories:
  - python
  - 技术杂谈
tags:
  - python
  - virtualenv
  - virtualenvwrapper
  - 虚拟环境
  - 隔离
---
## 关于：

VirtualEnv用于在一台机器上创建多个独立的python运行环境，VirtualEnvWrapper为前者提供了一些便利的命令行上的封装。

## 为什么要用

- 隔离项目之间的第三方包依赖，如A项目依赖django1.2.5，B项目依赖django1.3。

- 为部署应用提供方便，把开发环境的虚拟环境打包到生产环境即可,不需要在服务器上再折腾一翻。

## 怎么用

### 安装

- pip install virtualenvwrapper

- 把下面这句加到~/.bash_profile里面，如不嫌麻烦，也可以每次都手动执行。

source /usr/local/bin/virtualenvwrapper.sh

### 常用命令

创新的虚拟环境

<span style="color: #e52e00;"> &#8211; mkvirtualenv [env1]</span>

该命令会帮我们创建一个新环境,默认情况下，环境的目录是.virtualenv/en1,创建过程中它会自动帮我们安装pip，以后我们要安装新依赖时可直接使用pip命令。

创建完之后，自动切换到该环境下工作，可看到提示符变为：

(env1)$

在这个环境下安装的依赖不会影响到其他的环境

<span style="color: #e52e00;"> &#8211; lssitepackages</span>

显示该环境中所安装的包

切换环境

<span style="color: #e52e00;"> &#8211; workon [env]</span>

随时使用“workon 环境名”可以进行环境切换，如果不带环境名参数，则显示当前使用的环境

<span style="color: #e52e00;"> &#8211; deactivate</span>

在某个环境中使用，切换到系统的python环境

#### 其他命令

<span style="color: #e52e00;"> &#8211; showvirtualenv [env] </span>

显示指定环境的详情。

<span style="color: #e52e00;"> &#8211; rmvirtualenv [env] </span>

移除指定的虚拟环境，移除的前提是当前没有在该环境中工作。如在该环境工作，先使用deactivate退出。

<span style="color: #e52e00;"> &#8211; cpvirtualenv [source] [dest]</span>

复制一份虚拟环境。

<span style="color: #e52e00;"> &#8211; cdvirtualenv [subdir] </span>

把当前工作目录设置为所在的环境目录。

<span style="color: #e52e00;"> &#8211; cdsitepackages [subdir] </span>

把当前工作目录设置为所在环境的sitepackages路径。

<span style="color: #e52e00;"> &#8211; add2virtualenv [dir] [dir] </span>

把指定的目录加入当前使用的环境的path中，这常使用于在多个project里面同时使用一个较大的库的情况。

<span style="color: #e52e00;"> &#8211; toggleglobalsitepackages -q </span>

控制当前的环境是否使用全局的sitepackages目录。

<div id="xunlei_com_thunder_helper_plugin_d462f475-c18e-46be-bd10-327458d045bd">
</div>

转载请注明：[于哲的博客][1] &raquo; [【Python】使用VirtualEnvWrapper隔离python项目的库依赖][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/3194.html