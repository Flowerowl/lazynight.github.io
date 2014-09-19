---
title: 【Python】Monkey Patch
author: Flowerowl
layout: post
permalink: /3383.html
views:
  - 336
duoshuo_thread_id:
  - 1220743779864322565
categories:
  - python
tags:
  - monkey
  - patch
  - python
---
## What is it?

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  A <a href="https://web.archive.org/web/20120730014107/http://wiki.zope.org/zope2/MonkeyPatch">MonkeyPatch</a> is a piece of Python code which extends or modifies other code at runtime (typically at startup). MonkeyPatching<a class="new visualNoPrint" title="create this page" href="https://web.archive.org/web/20120730014107/http://wiki.zope.org/zope2/MonkeyPatch/createform?page=MonkeyPatching">?</a> would be the practice of writing, or running, a monkeypatch.
</p>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  A simple example looks like this:
</p>

<pre class="brush:applescript">from SomeOtherProduct.SomeModule import SomeClass
def speak(self): 
	return "ook ook eee eee eee!" 
SomeClass.speak = speak</pre>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  In this example, if SomeClass<a class="new visualNoPrint" title="create this page" href="https://web.archive.org/web/20120730014107/http://wiki.zope.org/zope2/MonkeyPatch/createform?page=SomeClass">?</a> did not already have a speak() method, it does now <img src='http://localhost/wordpress/wp-content/themes/d8/img/smilies/icon_smile.gif' alt=':-)' class='wp-smiley' /> If it had a speak() method before, the new code has replaced the old method definition.
</p>

<h2 style="margin-top: 1.5em; font-size: 21px; font-family: sans, sans-serif, verdana, arial, helvetica;">
  Why monkeypatch?
</h2>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  The motivation for monkeypatching is typically that the developer needs to modify or extend behavior of a third-party product, or Zope itself, and does not wish to maintain a private copy of the source code.
</p>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  For example, you may wish to add a tab to the Zope Management Interface screens for a core or third-party product. In Zope 2, there is no other easy way to do this short of forking the product. In Zope3, it is easier to reconfigure the system so we shouldn&#8217;t need to do this so often.
</p>

<h2 style="margin-top: 1.5em; font-size: 21px; font-family: sans, sans-serif, verdana, arial, helvetica;">
  Monkeypatching Considered Harmful
</h2>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  There are serious drawbacks to monkeypatching:
</p>

<ol style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  <li>
    If two modules attempt to monkeypatch the same method, one of them (whichever one runs last) &#8220;wins&#8221; and the other patch has no effect. (In some cases, if the &#8220;winning&#8221; monkeypatch takes care to call the original method, the other patch(es) may also work; but you must hope that the patches do not have contradictory intentions.)
  </li>
  <li>
    It creates a discrepancy between the original source code on disk and the observed behavior. This can be very confusing when troubleshooting, especially for anyone other than the monkeypatch&#8217;s author. Monkeypatching is therefore a kind of antisocial behavior.
  </li>
  <li>
    Monkeypatches can be a source of upgrade pain, when the patch makes assumptions about the patched object which are no longer true.
  </li>
</ol>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  So, just because python allows us to be very dynamic, it&#8217;s not always a good idea <img src='http://localhost/wordpress/wp-content/themes/d8/img/smilies/icon_smile.gif' alt=':)' class='wp-smiley' />
</p>

<h2 style="margin-top: 1.5em; font-size: 21px; font-family: sans, sans-serif, verdana, arial, helvetica;">
  Wherefore Art Thou Monkeypatch?
</h2>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  (add something about the history of the term here&#8230; there was a post about it maybe on zopezen.org a long time ago, but I can&#8217;t find it.)
</p>

<p style="font-family: sans, sans-serif, verdana, arial, helvetica;">
  <p>
    转载请注明：<a href="http://localhost/wordpress">于哲的博客</a> &raquo; <a href="http://localhost/wordpress/3383.html">【Python】Monkey Patch</a>
  </p>