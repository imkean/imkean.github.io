---
layout: page
date: 2016-07-20
title: errno
categories: clibrary
file_name: errno.h
tags: [errno]
---

**last error number**

该宏展开后是一个可修改的`int`类型的左值，因此可读可写。

`errno`在程序启动时被置零，任何C标准库的函数都可以修改该值，一般是改为对应的错误信号分类值（一旦该值被修改，库函数不会对其重置0)。

程序亦可修改该值。事实上，如果要在库函数调用后，使用该值检查错误，程序应该在调用前将该值重置为零。

## 相关非零常量宏

**EDOM(Domain error):** 值域错误，比如在调用`sqrt`时传入了负值参数，`errno`将被置为该值。

**ERANGE(Range Error):** 区间错误，比如在调用`pow`时计算的结果超出了浮点数的表示区间，`errno`则被设为该值。

**EILSEQ(Illegal sequence)** 字符序列非法错误，在处理多子节序列时，库函数遇到了非法序列时，`errno`则被设为该值。

标准库的函数可能将`errno`设置为任何值（以上常量宏之外的值)。

具体的库实现可能会在本头文件中附加定义别的宏。
