---
layout: post
title: Python学习笔记——可变对象和不可变对象
date: 2015/07/11 10:55:27
categories: 
- 技术
tags: 
- python
---

> python中，万物皆对象。

python中不存在所谓的传值调用，一切传递的都是对象的引用，也可以认为是传址。

## 一、可变对象和不可变对象

Python在heap中分配的对象分成两类：可变对象和不可变对象。所谓可变对象是指，对象的内容可变，而不可变对象是指对象内容不可变。

不可变（immutable）：int、字符串(string)、float、（数值型number）、元组（tuple)

可变（mutable）：字典型(dictionary)、列表型(list)

不可变类型特点：

```
   >>>i = 73  
   >>>i += 2 
```

![请输入图片描述][1]

从上图可知，不可变对象的特征没有变，变的只是创建了新对象，改变了变量的对象引用。

```
   >>>x = 1
   >>>y = 1
   >>>x = 1
   >>> x is y
   True
   >>>y is z
   True
```

如上所示，因为整数为不可变，x,y,z在内存中均指向一个值为1的内存地址，也就是说，x,y,z均指向的是同一个地址，值得注意的是，整形来说，目前仅支持(-1,100)。

总结一下，不可变对象的优缺点。

优点是，这样可以减少重复的值对内存空间的占用。

缺点呢，如例1所示，我要修改这个变量绑定的值，如果内存中没用存在该值的内存块，那么必须重新开辟一块内存，把新地址与变量名绑定。而不是修改变量原来指向的内存块的值，这回给执行效率带来一定的降低。

```
   m=[5,9]
   m+=[6]
```

![请输入图片描述][2]

## 二、函数参数：

Python函数参数对于可变对象，函数内对参数的改变会影响到原始对象；对于不可变对象，函数内对参数的改变不会影响到原始参数。原因在于：

1. 可变对象，参数改变的是可变对象，其内容可以被修改。
2. 不可变对象，改变的是函数内变量的指向对象。

 [1]: http://img.blog.csdn.net/20140902164438953

 [2]: http://img.blog.csdn.net/20140902165215796
