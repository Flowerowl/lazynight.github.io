---
title: 【Python】Python的functools
author: Flowerowl
layout: post
permalink: /3017.html
views:
  - 885
duoshuo_thread_id:
  - 1220743779864322497
bot_views:
  - 1
categories:
  - python
  - 代码
tags:
  - functools
  - python
  - wrapper
---
**【官方文档】**<a href="http://docs.python.org/2/library/functools.html#functools.partial" target="_blank">http://docs.python.org/2/library/functools.html#functools.partial</a>  
**【Lib/functools.py】**

<pre class="brush:applescript">"""functools.py - Tools for working with functions and callable objects
"""
# Python module wrapper for _functools C module
# to allow utilities written in Python to be added
# to the functools module.
# Written by Nick Coghlan &lt;ncoghlan at gmail.com&gt;
#   Copyright (C) 2006 Python Software Foundation.
# See C source code for _functools credits/copyright

from _functools import partial, reduce

# update_wrapper() and wraps() are tools to help write
# wrapper functions that can handle naive introspection

WRAPPER_ASSIGNMENTS = ('__module__', '__name__', '__doc__')
WRAPPER_UPDATES = ('__dict__',)
def update_wrapper(wrapper,
                   wrapped,
                   assigned = WRAPPER_ASSIGNMENTS,
                   updated = WRAPPER_UPDATES):
    """Update a wrapper function to look like the wrapped function

       wrapper is the function to be updated
       wrapped is the original function
       assigned is a tuple naming the attributes assigned directly
       from the wrapped function to the wrapper function (defaults to
       functools.WRAPPER_ASSIGNMENTS)
       updated is a tuple naming the attributes of the wrapper that
       are updated with the corresponding attribute from the wrapped
       function (defaults to functools.WRAPPER_UPDATES)
    """
    for attr in assigned:
        setattr(wrapper, attr, getattr(wrapped, attr))
    for attr in updated:
        getattr(wrapper, attr).update(getattr(wrapped, attr, {}))
    # Return the wrapper so this can be used as a decorator via partial()
    return wrapper

def wraps(wrapped,
          assigned = WRAPPER_ASSIGNMENTS,
          updated = WRAPPER_UPDATES):
    """Decorator factory to apply update_wrapper() to a wrapper function

       Returns a decorator that invokes update_wrapper() with the decorated
       function as the wrapper argument and the arguments to wraps() as the
       remaining arguments. Default arguments are as for update_wrapper().
       This is a convenience function to simplify applying partial() to
       update_wrapper().
    """
    return partial(update_wrapper, wrapped=wrapped,
                   assigned=assigned, updated=updated)

def total_ordering(cls):
    """Class decorator that fills in missing ordering methods"""
    convert = {
        '__lt__': [('__gt__', lambda self, other: not (self &lt; other or self == other)),
                   ('__le__', lambda self, other: self &lt; other or self == other),
                   ('__ge__', lambda self, other: not self &lt; other)],
        '__le__': [('__ge__', lambda self, other: not self &lt;= other or self == other),
                   ('__lt__', lambda self, other: self &lt;= other and not self == other),
                   ('__gt__', lambda self, other: not self &lt;= other)],
        '__gt__': [('__lt__', lambda self, other: not (self &gt; other or self == other)),
                   ('__ge__', lambda self, other: self &gt; other or self == other),
                   ('__le__', lambda self, other: not self &gt; other)],
        '__ge__': [('__le__', lambda self, other: (not self &gt;= other) or self == other),
                   ('__gt__', lambda self, other: self &gt;= other and not self == other),
                   ('__lt__', lambda self, other: not self &gt;= other)]
    }
    roots = set(dir(cls)) & set(convert)
    if not roots:
        raise ValueError('must define at least one ordering operation: &lt; &gt; &lt;= &gt;=')
    root = max(roots)       # prefer __lt__ to __le__ to __gt__ to __ge__
    for opname, opfunc in convert[root]:
        if opname not in roots:
            opfunc.__name__ = opname
            opfunc.__doc__ = getattr(int, opname).__doc__
            setattr(cls, opname, opfunc)
    return cls

def cmp_to_key(mycmp):
    """Convert a cmp= function into a key= function"""
    class K(object):
        __slots__ = ['obj']
        def __init__(self, obj, *args):
            self.obj = obj
        def __lt__(self, other):
            return mycmp(self.obj, other.obj) &lt; 0
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) &gt; 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) &lt;= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) &gt;= 0
        def __ne__(self, other):
            return mycmp(self.obj, other.obj) != 0
        def __hash__(self):
            raise TypeError('hash not implemented')
    return K</pre>

**【概念】**

The functools module is for higher-order functions: functions that act on or return other functions. In general, any callable object can be treated as a function for the purposes of this module.

**【内容】**

functools里边有partial，reduce，update_wrapper，wraps几个函数

[<img class="alignnone size-full wp-image-3018" alt="functools" src="http://lazynight.me/wp-content/uploads/2013/09/functools.gif" width="797" height="184" />][1]

**【解析】**

**1.partial**可以重新绑定函数的可选参数，生成一个callable的partial对象

partial主要是用来修改函数签名，使一些参数固化，以提供一个更简单的函数供以后调用

<pre class="brush:applescript">&gt;&gt;&gt; int('10') # 实际上等同于int('10', base=10)和int('10', 10) 这里base为10进制 
10  
&gt;&gt;&gt; int('10', 2) # 实际上是int('10', base=2)的缩写，二进制
2  
&gt;&gt;&gt; from functools import partial  
&gt;&gt;&gt; int2 = partial(int, 2) # 这里我没写base，结果就出错了  
&gt;&gt;&gt; int2('10')  
Traceback (most recent call last):  
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;  
TypeError: an integer is required  
&gt;&gt;&gt; int2 = partial(int, base=2) # 把base参数绑定在int2这个函数里  
&gt;&gt;&gt; int2('10') # 现在缺省参数base被设为2了  
2  
&gt;&gt;&gt; int2('10', 3) # 没加base，结果又出错了  
Traceback (most recent call last):  
  File "&lt;stdin&gt;", line 1, in &lt;module&gt;  
TypeError: keyword parameter 'base' was given by position and by name  
&gt;&gt;&gt; int2('10', base=3)  
3  
&gt;&gt;&gt; type(int2)  
&lt;type 'functools.partial'&gt;</pre>

从中可以看出，唯一要注意的是可选参数必须写出参数名

**2.reduce**,** **和python内置的reduce是一样的

[<img class="alignnone size-full wp-image-3019" alt="reduce" src="http://lazynight.me/wp-content/uploads/2013/09/reduce.gif" width="422" height="106" />][2]

**3.update_wrapper**

The main intended use for this function is in decorator functions which wrap the decorated function and return the wrapper. If the wrapper function is not updated, the metadata of the returned function will reflect the wrapper definition rather than the original function definition, which is typically less than helpful.

update\_wrapper是wraps的主要功能提供者，它负责考贝原函数的属性，默认是：&#8217;\\_\_module\_\_&#8217;, &#8216;\_\_name\_\_&#8217;, &#8216;\_\_doc\_\_&#8217;， &#8216;\_\_dict\_\_&#8217;。

update\_wrapper把被封装函数的\\_\_name\_\_、\_\_module\_\_、\_\_doc\_\_和 \_\_dict\_\_都复制到封装函数去

&nbsp;

**4.wrapper**

This is a convenience function for invoking <tt>partial(update_wrapper, wrapped=wrapped,assigned=assigned, updated=updated)</tt> as a function decorator when defining a wrapper function.

其实也就是相当于在调用了partial，使用wraps可以把整个function的属性都带上。

wraps主要是用来包装函数，使被包装含数更像原函数，它是对partial(update_wrapper, &#8230;)的简单包装

<pre class="brush:applescript">from functools import wraps
def my_decorator(f):
    @wraps(f)
    def wrapper(*args, **kwds):
        print 'Calling decorated function'
        return f(*args, **kwds)
    return wrapper

@my_decorator
def example():
    """Docstring"""
    print 'Called example function'
example()
print example.__name__
print example.__doc__</pre>

**结果**

<pre class="brush:applescript">Calling decorated function
Called example function
example
Docstring</pre>

&nbsp;

转载请注明：[于哲的博客][3] &raquo; [【Python】Python的functools][4]

 [1]: http://lazynight.me/wp-content/uploads/2013/09/functools.gif
 [2]: http://lazynight.me/wp-content/uploads/2013/09/reduce.gif
 [3]: http://localhost/wordpress
 [4]: http://localhost/wordpress/3017.html