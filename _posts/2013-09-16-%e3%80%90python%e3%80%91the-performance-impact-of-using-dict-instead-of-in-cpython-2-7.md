---
title: '【Python】The Performance Impact of Using dict() Instead of {} in CPython 2.7'
author: Flowerowl
layout: post
permalink: /2995.html
views:
  - 415
duoshuo_thread_id:
  - 1220743779864322491
bot_views:
  - 1
categories:
  - python
  - 实用
  - 技术杂谈
tags:
  - python
  - 分析
  - 源码
---
偶然在<a href="http://the5fire.com" target="_blank">胡阳</a>哥的博客里看见大神一篇文章，分析创建字典到底用dict()还是{}，这篇文章给出了详尽的分析。

看这种文章就是一种享受，必须要打印出来，慢慢看~哈哈

<div>
  <div id="the-performance-impact-of-using-dict-instead-of-in-cpython-2-7">
    <p>
      I’ve been reviewing lot of code lately for various open source and internal projects written in Python. As part of those reviews, I have noticed what I think is a trend toward using<tt>dict()</tt> instead of <tt>{}</tt> to create dictionaries. I don’t know exactly why this trend has emerged. Perhaps the authors perceive <tt>dict()</tt> as more readable than <tt>{}</tt>. Whatever the reason, my intuition told me calling the function version of the constructor for adictionary would impose a performance penalty. I studied what happens in both cases to understand how significant that penalty is, and the results confirmed my intuition.
    </p>
    
    <div id="tl-dr">
      <h2>
        tl;dr
      </h2>
      
      <p>
        With CPython 2.7, using <tt>dict()</tt> to create dictionaries takes up to 6 times longer and involves more memory allocation operations than the literal syntax. Use <tt>{}</tt> to create dictionaries, especially if you are pre-populating them, unless the literal syntax does not work for your case.
      </p>
    </div>
    
    <div id="initial-hypothesis">
      <h2>
        Initial Hypothesis
      </h2>
      
      <p>
        I wanted to study the performance difference between the literal syntax for creating a dictionary instance (<tt>{}</tt>) and using the name of the class to create one (<tt>dict()</tt>). I knew that the Python interpreter is based on opcodes and that there are codes dedicated to creating a dictionary that would not be invoked when the <tt>dict()</tt> form was used instead of the literal form. I suspected that the extra overhead for looking up the name <tt>"dict"</tt> and then calling the function would make the “function” form slower.
      </p>
    </div>
    
    <div id="measuring-performance">
      <h2>
        Measuring Performance
      </h2>
      
      <p>
        I began my analysis by applying <a href="http://www.doughellmann.com/PyMOTW/timeit/">timeit</a> to see if the performance difference was even measurable.
      </p>
      
      <div>
        <pre>$ python2.7 -m timeit -n 1000000 -r 5 -v 'dict()'
raw times: 0.24 0.24 0.24 0.239 0.24
1000000 loops, best of 5: 0.239 usec per loop

$ python2.7 -m timeit -n 1000000 -r 5 -v '{}'
raw times: 0.0417 0.0413 0.0407 0.0411 0.042
1000000 loops, best of 5: 0.0407 usec per loop</pre>
      </div>
      
      <p>
        Immediately I could see that not only was there a difference, it was even more significant than I expected. Using <tt>dict()</tt> to create an empty dictionary took 6 times longer than using the literal syntax. Would the difference be the same if the dictionary had members?
      </p>
      
      <p>
        Using dict() to create an empty dictionary took 6 times longer than using the literal syntax.
      </p>
      
      <div>
        <pre>$ python2.7 -m timeit -n 1000000 -r 5 -v 'dict(a="A", b="B", c="C")'
raw times: 0.56 0.548 0.563 0.563 0.556
1000000 loops, best of 5: 0.548 usec per loop

$ python2.7 -m timeit -n 1000000 -r 5 -v '{"a": "A", "b": "B", "c": "C"}'
raw times: 0.178 0.178 0.18 0.177 0.179
1000000 loops, best of 5: 0.177 usec per loop</pre>
      </div>
      
      <p>
        Passing a few members to the dictionary brought the difference closer together, but the function form was still taking three times as long.
      </p>
    </div>
    
    <div id="what-is-going-on">
      <h2>
        What is going on?
      </h2>
      
      <p>
        After establishing the performance difference, I asked myself what was going on to cause such a significant slowdown. To answer that question, I needed to look more deeply into what the interpreter was doing as it processed each expression. I wanted to see which (and how many) opcodes were being executed. I used <a href="http://www.doughellmann.com/PyMOTW/dis/">dis</a> to <em>disassemble</em> the Python expressions to see which opcodes implement each.
      </p>
      
      <p>
        To use dis from the command line, I needed input files containing the different expressions I was studying. I created <tt>func.py</tt>:
      </p>
      
      <div>
        <div>
          <pre>dict()</pre>
        </div>
      </div>
      
      <p>
        and <tt>literal.py</tt>:
      </p>
      
      <div>
        <div>
          <pre>{}</pre>
        </div>
      </div>
      
      <p>
        The output of dis is arranged in columns with the original source line number, the instruction “address” within the code object, the opcode name, and any arguments passed to the opcode.
      </p>
      
      <div>
        <pre>$ python2.7 -m dis func.py
  1           0 LOAD_NAME                0 (dict)
              3 CALL_FUNCTION            0
              6 POP_TOP
              7 LOAD_CONST               0 (None)
             10 RETURN_VALUE</pre>
      </div>
      
      <p>
        The function form uses two separate opcodes: <tt>LOAD_NAME</tt> to find the object associated with the name “dict”, and <tt>CALL_FUNCTION</tt> to invoke it. The last three opcodes are not involved in creating or populating the dictionary, and appear in both versions of the code, so I ignored them for my analysis.
      </p>
      
      <p>
        The literal form uses a special opcode to create the dictionary:
      </p>
      
      <div>
        <pre>$ python2.7 -m dis literal.py
  1           0 BUILD_MAP                0
              3 POP_TOP
              4 LOAD_CONST               0 (None)
              7 RETURN_VALUE</pre>
      </div>
      
      <p>
        The <tt>BUILD_MAP</tt> opcode creates a new empty dictionary instance and places it on the top of the interpreter’s stack.
      </p>
      
      <p>
        After comparing the two sets of opcodes, I suspected that the <tt>CALL_FUNCTION</tt> operation was the culprit, since calling functions is relatively expensive in Python. However, these were trivial examples and did not look like what I was seeing in code reviews. Most of the actual code I had seen was populating the dictionary as it created it, and I wanted to understand what difference that would make.
      </p>
    </div>
    
    <div id="examining-more-complex-examples">
      <h2>
        Examining More Complex Examples
      </h2>
      
      <p>
        I created two new source files that set three key/value pairs in the dictionary as it is created. I started with <tt>func-members.py</tt>, which instantiated a dictionary with the same members using the <tt>dict()</tt> function.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>dict(a="A",
     b="B",
     c="C",
     )</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        I realized that in order to really understand what was going on, I would have to look at the interpreter implementation.
      </p>
      
      <p>
        The disassembled version of <tt>func-members.py</tt> started the same way as the earlier example, looking for the <tt>dict()</tt> function:
      </p>
      
      <div>
        <pre>$ python2.7 -m dis func-members.py
  1           0 LOAD_NAME                0 (dict)</pre>
      </div>
      
      <p>
        Then it showed key/value pairs being pushed onto the stack using <tt>LOAD_CONST</tt> to create named arguments for the function.
      </p>
      
      <div>
        <pre>            3 LOAD_CONST               0 ('a')
            6 LOAD_CONST               1 ('A')
            9 LOAD_CONST               2 ('b')

2          12 LOAD_CONST               3 ('B')
           15 LOAD_CONST               4 ('c')

3          18 LOAD_CONST               5 ('C')</pre>
      </div>
      
      <p>
        Finally <tt>dict()</tt> was called:
      </p>
      
      <div>
        <pre>21 CALL_FUNCTION          768
24 POP_TOP
25 LOAD_CONST               6 (None)
28 RETURN_VALUE</pre>
      </div>
      
      <p>
        Next I created <tt>literal-members.py</tt>:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>{"a": "A",
 "b": "B",
 "c": "C",
 }</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The disassembled version of this example showed a few differences from the literal examplewithout any values. First, the argument to <tt>BUILD_MAP</tt> was 3 instead of 0, indicating that there were three key/value pairs on the stack to go into the dictionary.
      </p>
      
      <div>
        <pre>$ python2.7 -m dis literal-members.py
  1           0 BUILD_MAP                3</pre>
      </div>
      
      <p>
        It also showed the values and then keys being pushed onto the stack using <tt>LOAD_CONST</tt>.
      </p>
      
      <div>
        <pre>3 LOAD_CONST               0 ('A')
6 LOAD_CONST               1 ('a')</pre>
      </div>
      
      <p>
        Finally, a new opcode, <tt>STORE_MAP</tt>, appeared once after each key/value pair is processed.
      </p>
      
      <div>
        <pre>9 STORE_MAP</pre>
      </div>
      
      <p>
        The same pattern repeated for each of the other two key/value pairs.
      </p>
      
      <div>
        <pre>2          10 LOAD_CONST               2 ('B')
           13 LOAD_CONST               3 ('b')
           16 STORE_MAP

3          17 LOAD_CONST               4 ('C')
           20 LOAD_CONST               5 ('c')
           23 STORE_MAP
           24 POP_TOP
           25 LOAD_CONST               6 (None)
           28 RETURN_VALUE</pre>
      </div>
      
      <p>
        After looking at the output more closely, I noticed that there were actually <em>fewer</em> opcodes in the function form than the literal form. There were no <tt>STORE_MAP</tt> opcodes, just the <tt>CALL_FUNCTION</tt>after all of the items were on the stack. At this point I realized that in order to really understand what was going on, I would have to look at the interpreter implementation.
      </p>
    </div>
    
    <div id="interpreter-source">
      <h2>
        Interpreter Source
      </h2>
      
      <p>
        Up to now, I had been examining the behavior of the interpreter by feeding it inputs and seeing what it did with them. To examine it at a deeper level, I had to download the source following the instructions in the <a href="http://docs.python.org/devguide/">Dev Guide</a>.
      </p>
      
      <div>
        <pre>$ hg clone http://hg.python.org/cpython
destination directory: cpython
requesting all changes
adding changesets
adding manifests
adding file changes
added 80406 changesets with 178013 changes to 9805 files (+2 heads)
updating to branch default
3750 files updated, 0 files merged, 0 files removed, 0 files unresolved

$ cd cpython</pre>
      </div>
      
      <p>
        And, since I am looking at Python 2.7 rather than 3.3:
      </p>
      
      <div>
        <pre>$ hg update 2.7
3768 files updated, 0 files merged, 810 files removed, 0 files unresolved</pre>
      </div>
      
      <p>
        The interpreter evaluates opcodes in a loop defined in <tt>PyEval_EvalFrameEx()</tt> in <a href="http://hg.python.org/cpython/file/121872879e91/Python/ceval.c">Python/ceval.c</a>. Each opcode name corresponds to an entry in the <tt>switch</tt> statement. For example, the <tt>POP_TOP</tt>opcode that appears near the end of each disassembled example is implemented as:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case POP_TOP:
            v = POP();
            Py_DECREF(v);
            goto fast_next_opcode;</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The top-most item is removed from the stack and its reference count is decremented to allow it (eventually) to be garbage collected. After orienting myself in the source, I was ready to trace through the opcodes used in the examples above.
      </p>
    </div>
    
    <div id="what-happens-when-you-call-dict">
      <h2>
        What Happens When You Call dict()?
      </h2>
      
      <p>
        The disassembly above shows that the opcodes used to call <tt>dict()</tt> to create a dictionary are<tt>LOAD_NAME</tt>, <tt>LOAD_CONST</tt>, and <tt>CALL_FUNCTION</tt>.
      </p>
      
      <p>
        The <tt>LOAD_NAME</tt> opcode finds the object associated with the given name (“dict” in this case) and puts it on top of the stack.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case LOAD_NAME:
            w = GETITEM(names, oparg);
            if ((v = f-&gt;f_locals) == NULL) {
                PyErr_Format(PyExc_SystemError,
                             "no locals when loading %s",
                             PyObject_REPR(w));
                why = WHY_EXCEPTION;
                break;
            }
            if (PyDict_CheckExact(v)) {
                x = PyDict_GetItem(v, w);
                Py_XINCREF(x);
            }
            else {
                x = PyObject_GetItem(v, w);
                if (x == NULL && PyErr_Occurred()) {
                    if (!PyErr_ExceptionMatches(
                                    PyExc_KeyError))
                        break;
                    PyErr_Clear();
                }
            }
            if (x == NULL) {
                x = PyDict_GetItem(f-&gt;f_globals, w);
                if (x == NULL) {
                    x = PyDict_GetItem(f-&gt;f_builtins, w);
                    if (x == NULL) {
                        format_exc_check_arg(
                                    PyExc_NameError,
                                    NAME_ERROR_MSG, w);
                        break;
                    }
                }
                Py_INCREF(x);
            }
            PUSH(x);
            continue;</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        Three separate namespaces (represented as dictionaries) are searched. First, the local namespace from inside any function scope, followed by the module global namespace, and then the set of built-ins.
      </p>
      
      <p>
        <tt>LOAD_CONST</tt> is the next opcode used. It pushes literal constant values onto the interpreter’s stack:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4
5</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case LOAD_CONST:
            x = GETITEM(consts, oparg);
            Py_INCREF(x);
            PUSH(x);
            goto fast_next_opcode;</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The <tt>oparg</tt> value indicates which constant to take out of the set of constants found in the code object. The constant’s reference count is increased and then it is pushed onto the top of the stack. This is an inexpensive operation since no name lookup is needed.
      </p>
      
      <p>
        The portion of the implementation of <tt>CALL_FUNCTION</tt> in the case statement looks similarly simple:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case CALL_FUNCTION:
        {
            PyObject **sp;
            PCALL(PCALL_ALL);
            sp = stack_pointer;
#ifdef WITH_TSC
            x = call_function(&sp, oparg, &intr0, &intr1);
#else
            x = call_function(&sp, oparg);
#endif
            stack_pointer = sp;
            PUSH(x);
            if (x != NULL)
                continue;
            break;
        }</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The function is called and its return value is pushed onto the stack. (The <tt>WITH_TSC</tt> conditional compilation instruction controls whether the Pentium timestamp counter is used, and can be ignored for this general analysis.)
      </p>
      
      <p>
        The implementation of <tt>call_function()</tt> starts to expose some of the complexity of calling Python functions.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>static PyObject *
call_function(PyObject ***pp_stack, int oparg
#ifdef WITH_TSC
                , uint64* pintr0, uint64* pintr1
#endif
                )
{
    int na = oparg & 0xff;
    int nk = (oparg&gt;&gt;8) & 0xff;
    int n = na + 2 * nk;
    PyObject **pfunc = (*pp_stack) - n - 1;
    PyObject *func = *pfunc;
    PyObject *x, *w;

    /* Always dispatch PyCFunction first, because these are
       presumed to be the most frequent callable object.
    */
    if (PyCFunction_Check(func) && nk == 0) {
        int flags = PyCFunction_GET_FLAGS(func);
        PyThreadState *tstate = PyThreadState_GET();

        PCALL(PCALL_CFUNCTION);
        if (flags & (METH_NOARGS | METH_O)) {
            PyCFunction meth = PyCFunction_GET_FUNCTION(func);
            PyObject *self = PyCFunction_GET_SELF(func);
            if (flags & METH_NOARGS && na == 0) {
                C_TRACE(x, (*meth)(self,NULL));
            }
            else if (flags & METH_O && na == 1) {
                PyObject *arg = EXT_POP(*pp_stack);
                C_TRACE(x, (*meth)(self,arg));
                Py_DECREF(arg);
            }
            else {
                err_args(func, flags, na);
                x = NULL;
            }
        }
        else {
            PyObject *callargs;
            callargs = load_args(pp_stack, na);
            READ_TIMESTAMP(*pintr0);
            C_TRACE(x, PyCFunction_Call(func,callargs,NULL));
            READ_TIMESTAMP(*pintr1);
            Py_XDECREF(callargs);
        }
    } else {
        if (PyMethod_Check(func) && PyMethod_GET_SELF(func) != NULL) {
            /* optimize access to bound methods */
            PyObject *self = PyMethod_GET_SELF(func);
            PCALL(PCALL_METHOD);
            PCALL(PCALL_BOUND_METHOD);
            Py_INCREF(self);
            func = PyMethod_GET_FUNCTION(func);
            Py_INCREF(func);
            Py_DECREF(*pfunc);
            *pfunc = self;
            na++;
            n++;
        } else
            Py_INCREF(func);
        READ_TIMESTAMP(*pintr0);
        if (PyFunction_Check(func))
            x = fast_function(func, pp_stack, n, na, nk);
        else
            x = do_call(func, pp_stack, na, nk);
        READ_TIMESTAMP(*pintr1);
        Py_DECREF(func);
    }

    /* Clear the stack of the function object.  Also removes
       the arguments in case they weren't consumed already
       (fast_function() and err_args() leave them on the stack).
     */
    while ((*pp_stack) &gt; pfunc) {
        w = EXT_POP(*pp_stack);
        Py_DECREF(w);
        PCALL(PCALL_POP);
    }
    return x;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The number arguments the function is passed is given in <tt>oparg</tt>. The low-end byte is the number of positional arguments, and the high-end byte is the number of keyword arguments (lines 8-9). The value 768 in the example above translates to 3 keyword arguments and 0 positional arguments.
      </p>
      
      <div>
        <pre>21 CALL_FUNCTION          768</pre>
      </div>
      
      <p>
        There are separate cases for built-in functions implemented in C, function written in Python, and methods of objects. All of the cases eventually use <tt>load_args()</tt> to pull the positional arguments off of the stack as a tuple:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>static PyObject *
load_args(PyObject ***pp_stack, int na)
{
    PyObject *args = PyTuple_New(na);
    PyObject *w;

    if (args == NULL)
        return NULL;
    while (--na &gt;= 0) {
        w = EXT_POP(*pp_stack);
        PyTuple_SET_ITEM(args, na, w);
    }
    return args;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        And then <tt>update_keyword_args()</tt> is used to pull keyword arguments off of the stack <em>as a dictionary</em>:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>static PyObject *
update_keyword_args(PyObject *orig_kwdict, int nk, PyObject ***pp_stack,
                    PyObject *func)
{
    PyObject *kwdict = NULL;
    if (orig_kwdict == NULL)
        kwdict = PyDict_New();
    else {
        kwdict = PyDict_Copy(orig_kwdict);
        Py_DECREF(orig_kwdict);
    }
    if (kwdict == NULL)
        return NULL;
    while (--nk &gt;= 0) {
        int err;
        PyObject *value = EXT_POP(*pp_stack);
        PyObject *key = EXT_POP(*pp_stack);
        if (PyDict_GetItem(kwdict, key) != NULL) {
            PyErr_Format(PyExc_TypeError,
                         "%.200s%s got multiple values "
                         "for keyword argument '%.200s'",
                         PyEval_GetFuncName(func),
                         PyEval_GetFuncDesc(func),
                         PyString_AsString(key));
            Py_DECREF(key);
            Py_DECREF(value);
            Py_DECREF(kwdict);
            return NULL;
        }
        err = PyDict_SetItem(kwdict, key, value);
        Py_DECREF(key);
        Py_DECREF(value);
        if (err) {
            Py_DECREF(kwdict);
            return NULL;
        }
    }
    return kwdict;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        That’s right.
      </p>
      
      <p>
        In order to pass keyword arguments to dict() to instantiate a dictionary, first another dictionary is created.
      </p>
      
      <p>
        After the arguments are prepared, they are passed to <tt>dict()</tt>. The implementation of the dictionary object is found in <a href="http://hg.python.org/cpython/file/121872879e91/Objects/dictobject.c">Objects/dictobject.c</a>, and the <tt>dict</tt> type is defined as:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>PyTypeObject PyDict_Type = {
    PyVarObject_HEAD_INIT(&PyType_Type, 0)
    "dict",
    sizeof(PyDictObject),
    0,
    (destructor)dict_dealloc,                   /* tp_dealloc */
    (printfunc)dict_print,                      /* tp_print */
    0,                                          /* tp_getattr */
    0,                                          /* tp_setattr */
    (cmpfunc)dict_compare,                      /* tp_compare */
    (reprfunc)dict_repr,                        /* tp_repr */
    0,                                          /* tp_as_number */
    &dict_as_sequence,                          /* tp_as_sequence */
    &dict_as_mapping,                           /* tp_as_mapping */
    (hashfunc)PyObject_HashNotImplemented,      /* tp_hash */
    0,                                          /* tp_call */
    0,                                          /* tp_str */
    PyObject_GenericGetAttr,                    /* tp_getattro */
    0,                                          /* tp_setattro */
    0,                                          /* tp_as_buffer */
    Py_TPFLAGS_DEFAULT | Py_TPFLAGS_HAVE_GC |
        Py_TPFLAGS_BASETYPE | Py_TPFLAGS_DICT_SUBCLASS,         /* tp_flags */
    dictionary_doc,                             /* tp_doc */
    dict_traverse,                              /* tp_traverse */
    dict_tp_clear,                              /* tp_clear */
    dict_richcompare,                           /* tp_richcompare */
    0,                                          /* tp_weaklistoffset */
    (getiterfunc)dict_iter,                     /* tp_iter */
    0,                                          /* tp_iternext */
    mapp_methods,                               /* tp_methods */
    0,                                          /* tp_members */
    0,                                          /* tp_getset */
    0,                                          /* tp_base */
    0,                                          /* tp_dict */
    0,                                          /* tp_descr_get */
    0,                                          /* tp_descr_set */
    0,                                          /* tp_dictoffset */
    dict_init,                                  /* tp_init */
    PyType_GenericAlloc,                        /* tp_alloc */
    dict_new,                                   /* tp_new */
    PyObject_GC_Del,                            /* tp_free */
};</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        Because <tt>dict</tt> is a class, <tt>dict()</tt> creates a new object and then invokes the <tt>__init__()</tt> method. The initialization for <tt>dict</tt> is handled by <tt>dict_init()</tt>:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4
5</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>static int
dict_init(PyObject *self, PyObject *args, PyObject *kwds)
{
    return dict_update_common(self, args, kwds, "dict");
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        Which calls <tt>dict_update_common()</tt> to update the contents of the dictionary with the arguments passed to the initialization function.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>static int
dict_update_common(PyObject *self, PyObject *args, PyObject *kwds, char *methname)
{
    PyObject *arg = NULL;
    int result = 0;

    if (!PyArg_UnpackTuple(args, methname, 0, 1, &arg))
        result = -1;

    else if (arg != NULL) {
        if (PyObject_HasAttrString(arg, "keys"))
            result = PyDict_Merge(self, arg, 1);
        else
            result = PyDict_MergeFromSeq2(self, arg, 1);
    }
    if (result == 0 && kwds != NULL)
        result = PyDict_Merge(self, kwds, 1);
    return result;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        In this case, a set of keyword arguments are passed so the very last case is triggered and<tt>PyDict_Merge()</tt> is used to copy the keyword arguments into the dictionary. There are a couple of cases for merging, but from what I can tell because there are two dictionaries involved the first case applies. The target dictionary is resized to be big enough to hold the new values, and then the items from the merging dictionary are copied in one at a time.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>int
PyDict_Merge(PyObject *a, PyObject *b, int override)
{
    register PyDictObject *mp, *other;
    register Py_ssize_t i;
    PyDictEntry *entry;

    /* We accept for the argument either a concrete dictionary object,
     * or an abstract "mapping" object.  For the former, we can do
     * things quite efficiently.  For the latter, we only require that
     * PyMapping_Keys() and PyObject_GetItem() be supported.
     */
    if (a == NULL || !PyDict_Check(a) || b == NULL) {
        PyErr_BadInternalCall();
        return -1;
    }
    mp = (PyDictObject*)a;
    if (PyDict_Check(b)) {
        other = (PyDictObject*)b;
        if (other == mp || other-&gt;ma_used == 0)
            /* a.update(a) or a.update({}); nothing to do */
            return 0;
        if (mp-&gt;ma_used == 0)
            /* Since the target dict is empty, PyDict_GetItem()
             * always returns NULL.  Setting override to 1
             * skips the unnecessary test.
             */
            override = 1;
        /* Do one big resize at the start, rather than
         * incrementally resizing as we insert new items.  Expect
         * that there will be no (or few) overlapping keys.
         */
        if ((mp-&gt;ma_fill + other-&gt;ma_used)*3 &gt;= (mp-&gt;ma_mask+1)*2) {
           if (dictresize(mp, (mp-&gt;ma_used + other-&gt;ma_used)*2) != 0)
               return -1;
        }
        for (i = 0; i &lt;= other-&gt;ma_mask; i++) {
            entry = &other-&gt;ma_table[i];
            if (entry-&gt;me_value != NULL &&
                (override ||
                 PyDict_GetItem(a, entry-&gt;me_key) == NULL)) {
                Py_INCREF(entry-&gt;me_key);
                Py_INCREF(entry-&gt;me_value);
                if (insertdict(mp, entry-&gt;me_key,
                               (long)entry-&gt;me_hash,
                               entry-&gt;me_value) != 0)
                    return -1;
            }
        }
    }
    else {
        /* Do it the generic, slower way */
        PyObject *keys = PyMapping_Keys(b);
        PyObject *iter;
        PyObject *key, *value;
        int status;

        if (keys == NULL)
            /* Docstring says this is equivalent to E.keys() so
             * if E doesn't have a .keys() method we want
             * AttributeError to percolate up.  Might as well
             * do the same for any other error.
             */
            return -1;

        iter = PyObject_GetIter(keys);
        Py_DECREF(keys);
        if (iter == NULL)
            return -1;

        for (key = PyIter_Next(iter); key; key = PyIter_Next(iter)) {
            if (!override && PyDict_GetItem(a, key) != NULL) {
                Py_DECREF(key);
                continue;
            }
            value = PyObject_GetItem(b, key);
            if (value == NULL) {
                Py_DECREF(iter);
                Py_DECREF(key);
                return -1;
            }
            status = PyDict_SetItem(a, key, value);
            Py_DECREF(key);
            Py_DECREF(value);
            if (status &lt; 0) {
                Py_DECREF(iter);
                return -1;
            }
        }
        Py_DECREF(iter);
        if (PyErr_Occurred())
            /* Iterator completed, via error */
            return -1;
    }
    return 0;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
    </div>
    
    <div id="creating-a-dictionary-with">
      <h2>
        Creating a Dictionary with {}
      </h2>
      
      <p>
        The opcodes used to implement the literal examples are <tt>BUILD_MAP</tt>, <tt>LOAD_CONST</tt>, and <tt>STORE_MAP</tt>. I started with the first opcode, <tt>BUILD_MAP</tt>, which creates the dictionary instance:
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre>1
2
3
4
5</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case BUILD_MAP:
            x = _PyDict_NewPresized((Py_ssize_t)oparg);
            PUSH(x);
            if (x != NULL) continue;
            break;</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The dictionary is created using <tt>_PyDict_NewPresized()</tt>, from <a href="http://hg.python.org/cpython/file/121872879e91/Objects/dictobject.c">Objects/dictobject.c</a>.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>/* Create a new dictionary pre-sized to hold an estimated number of elements.
   Underestimates are okay because the dictionary will resize as necessary.
   Overestimates just mean the dictionary will be more sparse than usual.
*/

PyObject *
_PyDict_NewPresized(Py_ssize_t minused)
{
    PyObject *op = PyDict_New();

    if (minused&gt;5 && op != NULL && dictresize((PyDictObject *)op, minused) == -1) {
        Py_DECREF(op);
        return NULL;
    }
    return op;
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The argument for <tt>BUILD_MAP</tt> is the number of items that are going to be added to the new dictionary as it is created. The disassembly for <tt>literal-members.py</tt> showed that value as <tt>3</tt>earlier.
      </p>
      
      <div>
        <pre>$ python2.7 -m dis literal-members.py
  1           0 BUILD_MAP                3</pre>
      </div>
      
      <p>
        Specifying the initial number of items in the dictionary is an optimization for managing memory, since it means the table size can be set ahead of time and it does not need to be reallocated in some cases.
      </p>
      
      <p>
        The argument for BUILD_MAP is the number of items that are going to be added to the new dictionary as it is created.
      </p>
      
      <p>
        Each key/value pair is added to the dictionary using three opcodes. Two instances of<tt>LOAD_CONST</tt> push the value, then the key, onto the stack. Then a <tt>STORE_MAP</tt> opcode adds the pair to the dictionary.
      </p>
      
      <div>
        <pre>2          10 LOAD_CONST               2 ('B')
           13 LOAD_CONST               3 ('b')
           16 STORE_MAP</pre>
      </div>
      
      <p>
        As we saw early, <tt>LOAD_CONST</tt> is fairly straightforward and economical. <tt>STORE_MAP</tt> looks for the key, value, and dictionary on the stack and calls <tt>PyDict_SetItem()</tt> to add the new key/value pair. The <tt>STACKADJ(-2)</tt> line removes the key and value off from the stack.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>        case STORE_MAP:
            w = TOP();     /* key */
            u = SECOND();  /* value */
            v = THIRD();   /* dict */
            STACKADJ(-2);
            assert (PyDict_CheckExact(v));
            err = PyDict_SetItem(v, w, u);  /* v[w] = u */
            Py_DECREF(u);
            Py_DECREF(w);
            if (err == 0) continue;
            break;</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        <tt>PyDict_SetItem()</tt> is the same function invoked when a program uses <tt>d[k] = v</tt> to associate the value <tt>v</tt> with a key <tt>k</tt> in a dictionary.
      </p>
      
      <div>
        <table>
          <tr>
            <td>
              <div>
                <pre> 1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23</pre>
              </div>
            </td>
            
            <td>
              <div>
                <pre>int
PyDict_SetItem(register PyObject *op, PyObject *key, PyObject *value)
{
    register long hash;

    if (!PyDict_Check(op)) {
        PyErr_BadInternalCall();
        return -1;
    }
    assert(key);
    assert(value);
    if (PyString_CheckExact(key)) {
        hash = ((PyStringObject *)key)-&gt;ob_shash;
        if (hash == -1)
            hash = PyObject_Hash(key);
    }
    else {
        hash = PyObject_Hash(key);
        if (hash == -1)
            return -1;
    }
    return dict_set_item_by_hash_or_entry(op, key, hash, NULL, value);
}</pre>
              </div>
            </td>
          </tr>
        </table>
      </div>
      
      <p>
        The functions it calls handle resizing the internal data structure used by the dictionary, if that’s necessary.
      </p>
    </div>
    
    <div id="extreme-tests">
      <h2>
        Extreme Tests
      </h2>
      
      <p>
        I now had an answer explaining why the literal syntax was so much more efficient than using the name to instantiate a dictionary. I had noticed earlier, though, that the performance benefits were reduced when I added a few key/value pairs so I wanted to see if that trend continued as I added more arguments.
      </p>
      
      <p>
        I decided to jump right to the maximum point, and create dictionaries with 255 members. Because the number of keyword arguments passed to a function is represented in a byte, a function may be passed at most 255 literal keyword arguments (you can pass more arguments if you populate a dictionary and use the syntax <tt>callable(**kwds)</tt>, but 255 seemed like a reasonable number to test).
      </p>
      
      <p>
        Not wanting to type out all of those arguments, I created one script to generate a call to<tt>dict()</tt> with 255 arguments:
      </p>
      
      <div>
        <div>
          <pre>#!/usr/bin/env python

print 'dict(',
for i in range(255):
    print 'a%d=%d,' % (i, i),
print ')'</pre>
        </div>
      </div>
      
      <p>
        and another to create a literal dictionary with the same members:
      </p>
      
      <div>
        <div>
          <pre>#!/usr/bin/env python

print '{',
for i in range(255):
    print '"a%r": %r,' % (i, i),
print '}'</pre>
        </div>
      </div>
      
      <p>
        I then ran the scripts, and passed their output to <a href="http://www.doughellmann.com/PyMOTW/timeit/">timeit</a>. I used fewer iterations, since it took longer to run each one and process all of the arguments.
      </p>
      
      <div>
        <pre>$ python2.7 makebigfunc.py &gt; func-big.py
$ python2.7 -m timeit -n 10000 -r 5 -v "$(cat func-big.py)"
raw times: 0.214 0.211 0.211 0.216 0.221
10000 loops, best of 5: 21.1 usec per loop

$ python2.7 makebigdict.py &gt; literal-big.py
$ python2.7 -m timeit -n 10000 -r 5 -v "$(cat literal-big.py)"
raw times: 0.138 0.136 0.137 0.137 0.14
10000 loops, best of 5: 13.6 usec per loop</pre>
      </div>
      
      <p>
        The difference has narrowed, but using <tt>dict()</tt> still takes 1.6 times as long as <tt>{}</tt>.
      </p>
    </div>
    
    <div id="conclusions">
      <h2>
        Conclusions
      </h2>
      
      <p>
        In summary, calling <tt>dict()</tt> requires these steps:
      </p>
      
      <ol>
        <li>
          Find the object associated with the name <tt>"dict"</tt> and push it onto the stack.
        </li>
        <li>
          Push the key/value pairs onto the stack as constant values.
        </li>
        <li>
          Get the key/value pairs off of the stack and create a dictionary to hold the keyword arguments to the function.
        </li>
        <li>
          Call the constructor for <tt>dict</tt> to make a new object.
        </li>
        <li>
          Initialize the new object by passing the keyword arguments to its initialization method.
        </li>
        <li>
          Resize the new <tt>dict</tt> and copy the key/value pairs into it from the keyword arguments.
        </li>
      </ol>
      
      <p>
        Whereas using <tt>{}</tt> to create a dictionary uses only these steps:
      </p>
      
      <ol>
        <li>
          Create an empty but pre-allocated dictionary instance.
        </li>
        <li>
          Push the key/value pairs onto the stack as constant values.
        </li>
        <li>
          Store each key/value pair in the dictionary.
        </li>
      </ol>
      
      <p>
        The times involved here are pretty small, but as a general principle I try to avoid code constructions I know to introduce performance hits. On the other hand, there may be times when using <tt>dict()</tt> is necessary, or easier. For example, in versions of Python earlier than 2.7, creating a dictionary from an iterable required using a generator expression as argument to<tt>dict()</tt>:
      </p>
      
      <div>
        <div>
          <pre>d = dict((k, v) for k, v in some_iterable)</pre>
        </div>
      </div>
      
      <p>
        With 2.7 and later, though, <em>dictionary comprehensions</em> are built into the language syntax:
      </p>
      
      <div>
        <div>
          <pre>{k: v for k, v in some_iterable}</pre>
        </div>
      </div>
      
      <p>
        So that eliminates one common case for calling <tt>dict()</tt>. Using the literal dictionary syntax feels more “pythonic” to me, so I try to just do it anyway.
      </p>
      
      <p>
        转载自：<a href="http://doughellmann.com/2012/11/the-performance-impact-of-using-dict-instead-of-in-cpython-2-7-2.html">http://doughellmann.com/2012/11/the-performance-impact-of-using-dict-instead-of-in-cpython-2-7-2.html</a>
      </p>
    </div>
  </div>
</div>

转载请注明：[于哲的博客][1] &raquo; [【Python】The Performance Impact of Using dict() Instead of {} in CPython 2.7][2]

 [1]: http://localhost/wordpress
 [2]: http://localhost/wordpress/2995.html