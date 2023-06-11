---
title: 智能指针的使用以及实现
date: 2023-03-04 19:40:08
tags: 智能指针
categories: c++
cover: img/pexels-photo-2351286.jpeg
highlight_theme: mac
---

shared_ptr;

unique_ptr;

weak_ptr;

<!-- more -->     

## 1. shared_ptr

### 1.1 shared_ptr 类模板实现

```cpp
#include <iostream>
#include <mutex>
#include <thread>
using namespace std;

template <class T>
class Shared_Ptr {
 public:
  Shared_Ptr();
  // 拷贝构造函数
  Shared_Ptr(T *ptr = nullptr);

  // 析构函数
  ~Shared_Ptr();

  // 拷贝构造函数
  Shared_Ptr(const Shared_Ptr<T> &sp);

  // 重载赋值运算符 使得同一类型的shared_ptr智能指针可以相互赋值
  Shared_Ptr<T> &operator=(const Shared_Ptr<T> &sp);

  // 重载运算符 -> 当智能指针指向的数据类型为自定义的结构体时，通过 ->
  // 运算符可以获取其内部的指定成员
  T *operator->();

  // 重载 * 号，获取当前 shared_ptr 智能指针对象指向的数据
  T &operator*();

  // 获得引用计数
  int UseCount();

  // 返回 shared_ptr 对象内部包含的普通指针
  T *Get();

  // 引用计数增加
  void AddRefCount();

 private:
  void Release();
  int *_pRefCount;  // 引用计数指针
  T *_pPtr;         // 定义一个存储指针
  mutex *_pMutex;  // 定义锁指针，保证线程的安全，，防止资源未释放或程序崩溃
};

template <class T>
Shared_Ptr<T>::Shared_Ptr() {}

template <class T>
Shared_Ptr<T>::Shared_Ptr(T *ptr)
    : _pRefCount(new int(1)), _pPtr(ptr), _pMutex(new mutex) {}

template <class T>
Shared_Ptr<T>::~Shared_Ptr() {
  Release();
}
template <class T>
Shared_Ptr<T>::Shared_Ptr(const Shared_Ptr<T> &sp) {
  this->_pRefCount = sp._pRefCount;
  this->_pPtr = sp._pPtr;
  this->_pMutex = sp._pMutex;
  AddRefCount();
}

template <class T>
Shared_Ptr<T> &Shared_Ptr<T>::operator=(const Shared_Ptr<T> &sp) {
  if (_pPtr != sp._pPtr) {
    // 释放管理的旧资源
    // 当前对象所指向内存引用计数减一
    Release();
    // 共享管理新对象的资源，并增加引用计数
    _pPtr = sp._pPtr;
    _pRefCount = sp._pRefCount;
    _pMutex = sp._pMutex;
    AddRefCount();
  }
  return *this;
}

template <class T>
T *Shared_Ptr<T>::operator->() {
  return _pPtr;
}

template <class T>
T &Shared_Ptr<T>::operator*() {
  return *_pPtr;
}

template <class T>
int Shared_Ptr<T>::UseCount() {
  return *this->_pRefCount;
}

template <class T>
T *Shared_Ptr<T>::Get() {
  return this->_pPtr;
}

template <class T>
void Shared_Ptr<T>::AddRefCount() {
  _pMutex->lock();
  ++(*_pRefCount);
  _pMutex->unlock();
}

template <class T>
void Shared_Ptr<T>::Release() {
  bool deleteFlag = false;
  _pMutex->lock();
  if (--(*_pRefCount) == 0) {
    delete _pRefCount;
    _pRefCount = nullptr;
    delete _pPtr;
    _pPtr = nullptr;
    deleteFlag = true;
  }
  _pMutex->unlock();
  if (deleteFlag) {
    delete _pMutex;
  }
}
int main() {
  int *a = new int(3);
  int *b = new int(5);
  Shared_Ptr<int> p1(a);
  Shared_Ptr<int> p2(b);
  cout << *p1.Get() << " " << p1.UseCount() << endl;
  cout << *p2.Get() << " " << p2.UseCount() << endl;
  p2 = p1;
  cout << *p1.Get() << " " << p1.UseCount() << endl;
  cout << *p2.Get() << " " << p2.UseCount() << endl;
  system("pause");
  return 0;
}

/*
    3 1
    5 1
    3 2
    3 2
*/
```

.vscode-server/extensions/ms-vscode.cpptools-1.8.4/LLVM/bin



### 1.2 shared_ptr的简单使用

```cpp
	std::shared_ptr<int> ptra = std::make_shared<int>(20);
	std::shared_ptr<int> ptra1(new int(20));
```

前者`std::make_shared` 一次性为`int`对象和用于引用计数的数据都分配了内存；后者new操作符只是为int分配了内存，引用计数会在构造函数中再做处理。

注意： 后者千万不能写成：

`std::shared_ptr<int> ptra1 = new int(20);`
因为你将一个int指针赋值给一个类对象，肯定不行。其实很好理解，这种情况下我们要调用的是构造函数，而不是赋值构造。

而前者写成：

```cpp
std::shared_ptr<int> ptra(std::make_shared<int>(20));
```
**hared_ptr的常用成员函数**

```cpp
string* ps =  new string("hello")；
shared_ptr<string> p(ps);
```



| 成员函数      | 说明                                   |
| ------------- | -------------------------------------- |
| p.use_count() | 返回和p共享的类对象个数                |
| p.unique()    | 若p.use_count() == 1 时，返回true      |
| *p            | 等同 *ps，为hello                      |
| p->length()   | 等同ps->length()，返回string对象的长度 |
| p             | 若p指向一个对象，则返回true            |

**reset成员函数的使用**
尽管reset有4种形式，但下面只介绍基本的两种。

| 基本的两种形式                         | 说明                               |
| -------------------------------------- | ---------------------------------- |
| void reset() noexcept;                 | 将调用的对象清空，即变成一个空指针 |
| template < class T> void reset (T* p); | 使调用的shared_ptr获得p的所有权    |

```cpp
#include <iostream>
#include <string>
#include <memory>

using namespace std;

int main()
{
	std::shared_ptr<int> sp;  // empty
	sp.reset(new int);       // takes ownership of pointer
	*sp = 10;
	std::cout << *sp << '\n';

	sp.reset(new int);       // deletes managed object, acquires new pointer
	*sp = 20;
	std::cout << *sp << '\n';

	sp.reset();               // deletes managed object
	cout << sp.use_count() << endl;
	
	system("pause");
	return 0;
}
10
20
0
```

### 1.3 注意事项



**不要使用同一个原始指针构造 shared_ptr**
创建多个 shared_ptr 的正常方法是使用一个已存在的shared_ptr 进行创建，而不是使用同一个原始指针进行创建，如下所示：

```cpp
int *num = new int(23);
std::shared_ptr<int> p1(num);
std::shared_ptr<int> p2(p1);  // 正确使用方法
std::shared_ptr<int> p3(num); // 不推荐
```

在上面的使用中，num被p1和p3两个shared_ptr类管理，并且不像p1与p2关系是共享num一样，p1和p3是相互独立的，分别有独立的引用计数。这样当p1的引用计数为0而释放内存时，p3仍然指向那块被释放的内存。

**不要用栈中的指针构造 shared_ptr 对象**
shared_ptr 默认的构造函数中使用的是delete来删除关联的指针，所以构造的时候也必须使用new出来的堆空间的指针。
示例：

```cpp
   int x = 12;
   std::shared_ptr<int> ptr(&x);
   return 0;
```


当 shared_ptr 对象超出作用域调用析构函数delete 指针&x时会出错。

**建议使用 make_shared**
为了避免以上两种情形，建议使用make_shared()创建 shared_ptr 对象，而不是使用默认构造函数创建。

```cpp
std::shared_ptr<int> ptr_1 = make_shared<int>();
std::shared_ptr<int> ptr_2 (ptr_1);
```


另外不建议使用get()函数获取 shared_ptr 关联的原始指针，因为如果在 shared_ptr 析构之前手动调用了delete函数，同样会导致类似的错误。



## 2. unique_ptr

**unique_ptr禁止了拷贝构造和赋值构造,对某一个原始指针有“唯一”的管理权(独占所指向的对象);**

```cpp
unique_ptr<int> u(new int(5));
unique_ptr<int> u1;
```

由于*unique_ptr*禁止了拷贝构造和赋值构造，C++提供一个*move*函数，它的作用是移动。其实就和我们操作文件时一样，移动后源地址将清空。如下：

```cpp
unique_ptr<int> u(new int(5));
cout << *u << endl;
if (u)
    cout << "u is not empty" << endl;

unique_ptr<int> u1 = std::move(u);//将u移动给u1
cout << *u1 << endl;
if (!u)
    cout << "u is empty" << endl;

```

| 成员函数    | 说明                                                      |
| ----------- | --------------------------------------------------------- |
| u.release() | u释放对指针的控制权，并返回管理的指针，同时将u置空        |
| u.reset()   | 做两步动作，1是delete掉u管理的指针，2是将u置空            |
| u.reset(q)  | 做两步动作，1是delete掉u当前管理的指针，2是将q的控制权给u |

```cpp
int *pa = new int(5);
unique_ptr<int> u(pa);//u->pa

unique_ptr<int> u1(u.release());//u1->pa
cout << *u1 << endl;//打印结果为5

unique_ptr<int> u2(new int(10));//u2->new int(10)
u1.reset(u2.release());//u1->new int(10)，并且delete pa
cout << *pa << endl;//打印结果为任意未知数
cout << *u1 << endl;//打印结果为10

```

***unique_ptr*禁止了拷贝构造和赋值构造。但是它却可以`拷贝或赋值一个即将被销毁的*unique_ptr*对象`。最常见的就是函数内部的一个局部对象。**

```cpp
std::unique_ptr<int> clone(int p)
{
	return unique_ptr<int>(new int(p));
}

std::unique_ptr<int> clone2(int p)
{
	unique_ptr<int> ret(new int(p));
	
	return ret;
}

```

# 3. weak_ptr

weak_ptr是为了配合shared_ptr而引入的一种智能指针，因为它不具有普通指针的行为，没有重载operator*和->,它的最大作用在于协助shared_ptr工作，像旁观者那样观测资源的使用情况。

weak_ptr可以从一个shared_ptr或者另一个weak_ptr对象构造，获得资源的观测权。但weak_ptr没有共享资源，它的构造不会引起指针引用计数的增加。

使用weak_ptr的成员函数use_count()可以观测资源的引用计数，另一个成员函数expired()的功能等价于use_count()==0,但更快，表示被观测的资源(也就是shared_ptr的管理的资源)已经不复存在。

weak_ptr可以使用一个非常重要的成员函数lock()从被观测的shared_ptr获得一个可用的shared_ptr对象， 从而操作资源。但当expired()==true的时候，lock()函数将返回一个存储空指针的shared_ptr。

**使用weak_ptr解决循环引用的问题**

定义两个类，每个类中又包含一个指向对方类型的智能指针作为成员变量，然后创建对象，设置完成后查看引用计数后退出，看一下测试结果：

```cpp
class CB;
class CA
{
public:
    CA() { cout << "CA() called! " << endl; }
    ~CA() { cout << "~CA() called! " << endl; }
    void set_ptr(shared_ptr<CB>& ptr) { m_ptr_b = ptr; }
    void b_use_count() { cout << "b use count : " << m_ptr_b.use_count() << endl; }
    void show() { cout << "this is class CA!" << endl; }
private:
    shared_ptr<CB> m_ptr_b;
};

class CB
{
public:
    CB() { cout << "CB() called! " << endl; }
    ~CB() { cout << "~CB() called! " << endl; }
    void set_ptr(shared_ptr<CA>& ptr) { m_ptr_a = ptr; }
    void a_use_count() { cout << "a use count : " << m_ptr_a.use_count() << endl; }
    void show() { cout << "this is class CB!" << endl; }
private:
    shared_ptr<CA> m_ptr_a;
};

void test_refer_to_each_other()
{
    shared_ptr<CA> ptr_a(new CA());
    shared_ptr<CB> ptr_b(new CB());

    cout << "a use count : " << ptr_a.use_count() << endl;
    cout << "b use count : " << ptr_b.use_count() << endl;

    ptr_a->set_ptr(ptr_b);
    ptr_b->set_ptr(ptr_a);

    cout << "a use count : " << ptr_a.use_count() << endl;
    cout << "b use count : " << ptr_b.use_count() << endl;
}

/*
CA() called!
CB() called!
a use count : 1
b use count : 1
a use count : 2
b use count : 2
*/
```

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202305012054117.png)

>通过结果可以看到，最后CA和CB的对象并没有被析构，其中的引用效果如下图所示，起初定义完ptr_a和ptr_b时，只有①③两条引用，
>
>然后调用函数set_ptr后又增加了②④两条引用，当test_refer_to_each_other这个函数返回时，对象ptr_a和ptr_b被销毁，也就是①③两条引用会被断开，
>
>但是②④两条引用依然存在，每一个的引用计数都不为0，结果就导致其指向的内部对象无法析构，造成内存泄漏。
>



解决这种状况的办法就是将两个类中的一个成员变量改为`weak_ptr`对象，因为`weak_ptr`不会增加引用计数，使得引用形不成环，最后就可以正常的释放内部的对象，不会造成内存泄漏，比如将`CB`中的成员变量改为`weak_ptr`对象，代码如下

```cpp
class CB
{
public:
    CB() { cout << "CB() called! " << endl; }
    ~CB() { cout << "~CB() called! " << endl; }
    void set_ptr(shared_ptr<CA>& ptr) { m_ptr_a = ptr; }
    void a_use_count() { cout << "a use count : " << m_ptr_a.use_count() << endl; }
    void show() { cout << "this is class CB!" << endl; }
private:
    weak_ptr<CA> m_ptr_a;
};

CA() called!
CB() called!
a use count : 1
b use count : 1
a use count : 1
b use count : 2
~CA() called!
~CB() called!

```

通过这次结果可以看到，CA和CB的对象都被正常的析构了，引用关系如下图所示，流程与上一例子相似，但是不同的是④这条引用是通过weak_ptr建立的，并不会增加引用计数，也就是说CA的对象只有一个引用计数，而CB的对象只有2个引用计数，当test_refer_to_each_other这个函数返回时，对象ptr_a和ptr_b被销毁，也就是①③两条引用会被断开，此时CA对象的引用计数会减为0，对象被销毁，其内部的m_ptr_b成员变量也会被析构，导致CB对象的引用计数会减为0，对象被销毁，进而解决了引用成环的问题。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/202303091116170.png)