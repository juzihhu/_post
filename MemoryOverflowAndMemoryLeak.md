---
title: 内存溢出与内存泄漏
date: 2022-09-04 10:24:11
tags: 内存泄漏
categories: c++
cover: /img/SplitShire-VS-Developer-0137.jpg
---

<!-- more -->         

## 1. 内存溢出

内存溢出 OOM （out of memory），是指程序在申请内存时，没有足够的内存空间供其使用，出现out of memory；比如申请了一个int,但给它存了long才能存下的数，那就是内存溢出。

**导致内存溢出原因可能有：**

- 内存泄漏
- 超大对象
- 其他原因如死循环等

## 2. 内存泄漏

内存泄露 memory leak，是指程序在申请内存后，无法释放（或者忘记释放）已申请的内存空间，导致所申请的那块内存无法再访问；一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光。最终的结果就是导致OOM。

## 3. 造成内存泄露常见的三种情况

1，指针重新赋值

2，错误的内存释放

3，返回值的不正确处理

**3.1 指针重新赋值**

如下代码：

```cpp
char * p = (char *)malloc(10);
char * np = (char *)malloc(10);
```

其中，指针变量 p 和 np 分别被分配了 10 个字节的内存。

如果程序需要执行如下赋值语句：

```cpp
p=np;
```

这时候，指针变量 p 被 np 指针重新赋值，其结果是 p 以前所指向的内存位置变成了孤立的内存。它无法释放，因为没有指向该位置的引用，从而导致 10 字节的内存泄漏。

因此，在对指针赋值前，一定确保内存位置不会变为孤立的。

类似的情况，连续重复new的情况也是类似：

```cpp
 int *p = new int; 
 p = new int...;//错误
```

**3.2 错误的内存释放**

```cpp
int *p = new int;
int t = 3;
p = &t;
delete p;//delete掉的是栈上的内存，而堆上的内存再也无法释放。
```

**3.3 返回值的不正确处理**

有时候，某些函数会返回对动态分配的内存的引用，如下面的示例代码所示：

```text
char *f(){
	return (char *)malloc(10);
}
void f1(){
	f();
}
```

函数 f1 中对 f 函数的调用并未处理该内存位置的返回地址，其结果将导致 f 函数所分配的 10 个字节的块丢失，并导致内存泄漏。

**4 在内存分配后忘记使用 free 进行释放**