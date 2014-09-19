---
title: Python 备份小程序
author: Flowerowl
layout: post
permalink: /2395.html
duoshuo_thread_id:
  - 6431968
views:
  - 1033
bot_views:
  - 2
categories:
  - python
  - 代码
---
[<img class="alignnone size-full wp-image-2396" title="python" src="http://lazynight.me/wp-content/uploads/2012/08/python.jpg" alt="" width="517" height="159" />][1]

<pre class="lang:default decode:true">#!/usr/bin/python
# Filename: backup_ver4.py
import os
import time
# 1. The files and directories to be backed up are specified in a list.
source = ['/home', '/home/']
# If you are using Windows, use source = [r'C:\Documents', r'D:\Work'] or something like that
# 2. The backup must be stored in a main backup directory
target_dir = '/users/flowerowl/' # Remember to change this to what you will be using
# 3. The files are backed up into a zip file.
# 4. The current day is the name of the subdirectory in the main directory
today = target_dir + time.strftime('%Y%m%d')
# The current time is the name of the zip archive
now = time.strftime('%H%M%S')
# Take a comment from the user to create the name of the zip file
comment = raw_input('Enter a comment --&gt; ')
if len(comment) == 0: # check if a comment was entered
	target = today + os.sep + now + '.zip'
else:
	target = today + os.sep + now + '_' + \
comment.replace(' ', '_') + '.zip'
# Notice the backslash!
# Create the subdirectory if it isn't already there
if not os.path.exists(today):
	os.mkdir(today) # make directory
	print 'Successfully created directory', today
# 5. We use the zip command (in Unix/Linux) to put the files in a zip archive
zip_command = "zip -qr '%s' %s" % (target, ' '.join(source))
# Run the backup
if os.system(zip_command) == 0:
	print 'Successful backup to', target
else:
	print 'Backup FAILED'</pre>

&nbsp;

转载请注明：[于哲的博客][2] &raquo; [Python 备份小程序][3]

 [1]: http://lazynight.me/wp-content/uploads/2012/08/python.jpg
 [2]: http://localhost/wordpress
 [3]: http://localhost/wordpress/2395.html