---
layout: page
date: 2016-07-20
title: assert
categories: clibrary
file_name: assert.h
tags: [assert]
---

* Contents
{:toc #toc_of_keans_blog}

## assert

<pre>void assert (int expression); </pre>

**Evaluate assertion**

如果assert宏的参数*expression*的值为假，将会向标准错误设备输出一条出错信息，并调用*abort*终止程序执行。

出错消息的详细内容和标准库的具体实现相关，一般的出错信息的格式如下：

<pre>
Assertion failed: expression, file filename, line line number
</pre>

如果在引入头文件名``<assert.h>``之前定义`#define NDEBUG`，将会使所有`assert`语句失效。

一般来说调试环节结束后assert会被禁用掉，所以这个宏是被设计用来捕捉编程过程中的错误的，而不是用户错误或者运行时错误。

## Parameters

**expression**
对expression表达式求值，如果表达式为值0，则断言失败，终止程序。

## Return Value

none

## Example

<pre>
/* assert example */
#include <stdio.h>      /* printf */
#include <assert.h>     /* assert */

void print_number(int* myInt) {
  assert (myInt!=NULL);
  printf ("%d\n",*myInt);
}

int main ()
{
  int a=10;
  int * b = NULL;
  int * c = NULL;

  b=&a;

  print_number (b);
  print_number (c);

  return 0;
}
</pre>
