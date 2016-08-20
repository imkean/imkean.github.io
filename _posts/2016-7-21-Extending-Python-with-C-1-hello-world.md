---
layout: post
title: Extending Python with C or C++ (1) | Hello, Wolrd!
categories: [Python, Programming]
tags: [Python, C]
excerpt: "Python最为当今最流行的脚本语言之一，一方面轮子众多，用起来非常酸爽；另一方面，它被称为胶水语言，可以和C，C++，Java等语言进行混合编程，本文简单介绍了如何使用C给Python写一个Hello,Word扩展。"
---

* Contents
{:toc #toc_of_keans_blog}

Python的轮子众多，实际上其中大部分的轮子为了保证效率，都是使用C/C++实现的。创建Python扩展的步骤如下：

## 创建扩展代码

创建文件 **HelloWorld.c**

```c
#include <stdio.h>

int main()
{
    puts("Hello, World!");
    return 0;
}
```

使用gcc编译运行看看是否有错误：
<pre>
$ gcc HelloWorld.c -o hw
$ ./hw
Hello, World!
</pre>

## 使用模板来封装代码

**1. 引入Python头文件**

这个文件一般在``/usr/local/include/python2.x``
在*HelloWorld.c*文件中第一行加入 `#include <Python.h>`

**2. 为模块的每一个函数增加一个型如 static PyObject* Module_func()的包装函数**

包装函数的主要作用就是与Python进行交互，一般就是从Python里取数据，将Python类型的数据转化成C类型的数据，然后交给C模块处理数据，然后再将结果转换成Python类型的数据返回。为main函数添加如下包装函数

```c
static PyObject * HelloWorld_main(PyObject * self, PyObject * args) {
    main();
    return (PyObject*) Py_BuildValue("");
}
```

因为这个Hello，World函数太简单，不需要传入参数，但是返回值总是需要的，直接返回NULL会产生一个`TypeError`,对于我们这里的情形需要返回一个None，即在Python里面表示没有返回值。`Py_BuildValue`将C的类型转换为Python的类型，用法如下：

<pre>
Py_BuildValue("")                        None
Py_BuildValue("i", 123)                  123
Py_BuildValue("iii", 123, 456, 789)      (123, 456, 789)
Py_BuildValue("s", "hello")              'hello'
Py_BuildValue("ss", "hello", "world")    ('hello', 'world')
Py_BuildValue("s#", "hello", 4)          'hell'
Py_BuildValue("()")                      ()
Py_BuildValue("(i)", 123)                (123,)
Py_BuildValue("(ii)", 123, 456)          (123, 456)
Py_BuildValue("(i,i)", 123, 456)         (123, 456)
Py_BuildValue("[i,i]", 123, 456)         [123, 456]
Py_BuildValue("{s:i,s:i}",
              "abc", 123, "def", 456)    {'abc': 123, 'def': 456}
Py_BuildValue("((ii)(ii)) (ii)",
              1, 2, 3, 4, 5, 6)          (((1, 2), (3, 4)), (5, 6))
</pre>

**3. 在模块中增加一个PyMethodDef类型的静态数组**
这个数组的作用是在Python解释器导入的时候使用
格式如下：每一个数组都包含一个函数的信息，最后一个数组放置两个NULL值，代表声明结束

```c
static PyMethodDef methods[] = {
    {"main", HelloWorld_main, METH_VARARGS},
    {NULL, NULL},
};
```

METH_VARARGS代表参数以tuple的形式传入。如果我们需要使用PyArg_ParseTupleAndKeywords()
函数来分析关键字参数的话，这个标志常量应该写成: METH_VARARGS & METH_KEYWORDS，进行逻辑与运算。

**增加模块初始化函数void initMethod()**

最后的工作就是模块的初始化工作。这部分代码在模块被python导入时进行调用。
void initHelloWorld(){
    Py_InitModule("HelloWorld",methods);
 }

 最终 **HelloWorld.c** 全部代码如下：

```c
#include <Python.h>
#include <stdio.h>

int main()
{
	puts("Hello, Wolrd!");
	return 0;
}

static PyObject *
HelloWorld_main(PyObject * self, PyObject * args) {
	main();
	return (PyObject*) Py_BuildValue("");
}

static PyMethodDef methods[] = {
	{"main", HelloWorld_main, METH_VARARGS},
	{NULL, NULL},
};

void initHelloWorld(){
	Py_InitModule("HelloWorld",methods);
}
```

## 编译代码

**创建setup.py**

我们在安装python第三方包的时候，很多情况下会用到python setup.py install这个命令，
下面我们来了解一下setup.py文件的内容。

编译的最主要的内容由setup函数完成，你需要为每一个扩展创建一个Extension实例，在这里我们只有一个
扩展，所以只需要创建一个实例。
Extension('Extest', sources=['Extest.c'])，第一个参数是扩展的名字，如果模块是包的一部分，还需要加"."；
第二个参数是源代码文件列表
setup('Extest', ext_modules=[...])，第一个参数表示要编译哪个东西，第二个参数列出要编译的Extension对象

```python
#!/usr/bin/python
from distutils.core import setup, Extension

MOD_NAME = "HelloWorld"
setup(name = MOD_NAME, ext_modules = [Extension(MOD_NAME, sources = ["HelloWorld.c"])])

```

**通过运行setup.py来编译和连接你的代码**

通过一下命令编译模块，并安装模块

<pre>
$ python setup.py build
$ python setup.py install
</pre>

然后在Python使用模块

<pre>
Python 2.7.10 (default, Oct 23 2015, 19:19:21)
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import HelloWorld
>>> HelloWorld.main()
Hello, Wolrd!
>>>
</pre>

**或者不安装进行调试**

```python
#!/usr/bin/python
from ctypes import *
import os
#需要使用绝对路径
module = cdll.LoadLibrary(os.getcwd() + '/HelloWorld.so')
module.main()

```
