---
title: cpp设计模式总结
date: 2022-09-06 14:24:57
tags: 设计模式
categories: c++
cover: img/photo-1676735296710-758d95e3044f.jpg
---

## 1. 单例模式

> 定义：无论创建多少次对象，保证一个类仅有一个实例，并提供一个访问它的全局访问点，该实例被所有程序模块共享。

**核心思想：**

单例模式的核心是构造方法的私有化（即在入口处限制了对象的实例化），之后在类的内部实例化对象，并通过静态方法返回实例化对象的引用

**实现方式：**

- 懒汉式
- 饿汉式

**(1)饿汉式单例模式**：还没有获取实例对象，实例对象就已经产生了

```cpp
#include<iostream>

using namespace std;

class Singleton {
public:
    // 由于普通成员方法的调用依赖对象，而获取唯一对象的时候并没有产生对象，所以定义成static
    // static接口获取指向对象的指针
    static Singleton* getInstance() {
        return &instance;
    }

private:
    // 定义一个唯一的实例对象(数据段)
    // 由于是类Singleton内再写一个Singleton，这里被编译器处理成instance的类型声明，而非instance的定义，所以需要在类外初始化
    static Singleton instance;

    // 构造函数私有化
    Singleton() {

    }

    // delete拷贝构造和赋值重载
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

// 类外初始化static成员变量处于数据段，对象在函数执行前就存在，运行到的时候才初始化
Singleton Singleton::instance;

int main(){
    Singleton* p1 = Singleton::getInstance();
    Singleton* p2 = Singleton::getInstance();
    Singleton* p3 = Singleton::getInstance();
    cout << p1 << endl;
    cout << p2 << endl;
    cout << p3 << endl;
    return 0;
}
```

**获取唯一的实例对象的getInstance()方法定义成static**：`static Singleton* getInstance()`

普通成员方法的调用依赖对象，静态方法可以通过类名调用，而获取唯一对象的时候并没有产生对象，所以定义成static

**实例对象是static**: `static Singleton instance;`

存放在数据段，不占用对象空间。如果不写成static，会引起类的循环定义

**优点：线程安全**。对象存放在**数据段**，main函数还没有开始执行，对象就已经初始化好了，一定是[线程安全](https://so.csdn.net/so/search?q=线程安全&spm=1001.2101.3001.7020)的。

**缺点：** 获取在软件启动的时候，并没有使用到这个对象，然而这个对象已经产生，这就比较浪费资源。

**(2) 懒汉式单例模式**：唯一的实例对象是在第一次获取的时候才产生

instance = new Singleton()`被拆解为：开辟内存、构造对象、给instance赋值。只要还没给instance赋值，if条件满足，另一线程就可以进入if语句，**线程不安全**，不是可重入函数。

通过 **锁 + 双重判断** 的方法，实现线程安全的函数;

```cpp
class Singleton {
public:
    static Singleton* getInstance() {
       if (instance == nullptr) {
            unique_lock<mutex> lock(mtx);
            if (instance == nullptr) {
                instance = new Singleton();// 堆区
            }
        }
        return instance;
    }

private:
    // 定义一个唯一的实例对象的指针（数据段）
    static Singleton* volatile instance;

    Singleton() {}

    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

Singleton* volatile Singleton::instance = nullptr;
```

`instance`指针属于数据段，是同一进程多个进程共享的内存。
为了加快指令的执行，CPU会把共享内存的值拷贝一份带到各个线程的缓存，这依然导致线程不安全，所以加上`volatile`，使得各个线程不再持有缓存

```cpp
class Singleton {
public:
    static Singleton* getInstance() {
        // 函数局部静态变量的初始化，在汇编指令上已经自动添加线程互斥的指令
        static Singleton instance;
        return &instance;
    }

private:
    Singleton() {

    }

    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};
```

## 2. 简单工厂

> 在C++项目里会出现很多类，每次创建对象的时候，都需要通过**new 类名称**的方式来生成对象，用户需要记忆很多类的名称，这样的设计使得代码很难维护，类名如果做了改变，那么所有使用类名称的地方都需要去修改，耦合性太强，Simple Factory就此诞生的。

```cpp
#include<iostream>
#include<string>
#include<memory>

using namespace std;

class Car {
public:
    Car(string name) :_name(name) {

    }
    // 纯虚函数
    virtual void show() = 0;
    string _name;
};

class Bmw : public Car {
public:
    Bmw(string name) : Car(name) {

    }
    void show() {
        cout << "获取一辆BMW " <<_name<< endl;
    }
};

class Benz : public Car {
public:
    Benz(string name) : Car(name) {

    }
    void show() {
        cout << "获取一辆Benz " << _name << endl;
    }
};

enum CarType {
    BMW,
    BENZ
};

class SimpleFactory {
public:
    Car* create_car(CarType ct) {
        switch (ct) {
            case BMW:
                return new Bmw("X5");
            case BENZ:
                return new Benz("A6");
            default:
                cerr << "参数有误" << endl;
                break;
        }
        return nullptr;
    }
};

int main(){
    unique_ptr<SimpleFactory> factory(new SimpleFactory());
    unique_ptr<Car> p1(factory->create_car(BMW));
    unique_ptr<Car> p2(factory->create_car(BENZ));

    p1->show();
    p2->show();

    return 0;
}
```

create_car函数无法做到 **“开-闭”原则** ，即对原有代码修改关闭，对功能扩展开放。这个函数随着新对象的添加，或者原有对象的删除，都会导致该函数的代码修改，而且有可能影响原来的功能。

## 3. 工厂方法

工厂方法为每种产品都提供了相应的实例工厂，当增加新的汽车工厂时，只需要增加对应工厂类即可，删除汽车工厂时，则只需要删除工厂类。

可解决简单工厂不符合 **“开-闭”原则** 的问题

```cpp
class Benz : public Car {
public:
    Benz(string name) : Car(name) {

    }
    void show() {
        cout << "获取一辆Benz " << _name << endl;
    }
};

class Factory {
public:
    // 工厂方法
    virtual Car* create_car(string name) = 0;
};

// 宝马工厂
class BmwFactory : Factory{
public:
    Car* create_car(string name) {
        return new Bmw(name);
    }
};

// 奔驰工厂
class BenzFactory : Factory {
public:
    Car* create_car(string name) {
        return new Benz(name);
    }
};

int main(){
    // 创建工厂
    unique_ptr<BmwFactory> bmw_factory(new BmwFactory());
    unique_ptr<BenzFactory> benz_factory(new BenzFactory());

    // 给型号即可拿到车，无需手动new创建
    unique_ptr<Car> p1(bmw_factory->create_car("X5"));
    unique_ptr<Car> p2(benz_factory->create_car("A6"));

    p1->show();
    p2->show();

    return 0;
}
```

**缺点：** 每一个实例工厂负责生产一种实例产品，也就是**一个产品对应一个工厂，一个工厂对应一个产品**。在现实生活中一个工厂是可以生产多种产品的，而工厂方法中，**给这每一个产品都创建一个工厂类，这样代码中的类就太多了**，不好维护。

## 4. 抽象工厂

抽象工厂用于生产有关联关系的系列产品

```cpp
class Car {
public:
    Car(string name) :_name(name) {

    }
    // 纯虚函数
    virtual void show() = 0;
    string _name;
};

class Bmw : public Car {
public:
    Bmw(string name) : Car(name) {

    }
    void show() {
        cout << "获取一辆BMW " <<_name<< endl;
    }
};

class Benz : public Car {
public:
    Benz(string name) : Car(name) {

    }
    void show() {
        cout << "获取一辆Benz " << _name << endl;
    }
};
```

```cpp
class Light {
public:
    virtual void show() = 0;
};

class BmwLight : public Light{
public:
    void show() {
        cout << "得到一个BMW车灯" << endl;
    }
};

class BenzLight : public Light {
public:
    void show() {
        cout << "得到一个Benz车灯" << endl;
    }
};
```

****抽象工厂：对有一组关联关系的产品簇提供统一创建****

```cpp
class AbstractFactory {
public:
    // 工厂方法 创建汽车
    virtual Car* create_car(string name) = 0;
    // 工厂方法 创建车灯
    virtual Light* create_car_light() = 0;
};

// BmwFactory
class BmwFactory : public AbstractFactory {
public:
    Car* create_car(string name) {
        return new Bmw(name);
    }

    Light* create_car_light() {
        return new BmwLight();
    }
};

// BenzFactory
class BenzFactory : public AbstractFactory {
public:
    Car* create_car(string name) {
        return new Benz(name);
    }

    Light* create_car_light() {
        return new BenzLight();
    }
};

int main(){
    // 创建抽象工厂，可生产有关联关系的一类产品
    unique_ptr<AbstractFactory> bmw_factory(new BmwFactory());
    unique_ptr<AbstractFactory> benz_factory(new BenzFactory());

    unique_ptr<Car> c1(bmw_factory->create_car("X5"));
    unique_ptr<Car> c2(benz_factory->create_car("A6"));

    unique_ptr<Light> l1(bmw_factory->create_car_light());
    unique_ptr<Light> l2(benz_factory->create_car_light());

    c1->show();
    c2->show();
    l1->show();
    l2->show();

    return 0;
}
```

**简单工厂 Simple Factory**

把对象的创建封装在一个接口函数里面，通过传入不同的标识，返回不同的对象。客户不用自己手动new对象，不用了解对象的创建过程。

提供创建对象实例的接口函数不闭合，不能对修改关闭

**工厂方法 Factory Method**

工厂基类，提供了纯虚函数用于提供创建产品的接口，派生类重写创建产品的接口。可以做到不同的产品在不同的工厂里创建，模块化清晰。

可以做到对现有工厂以及产品的修改关闭。

实际上，很多有关联关系的产品是放在同一工厂生产的。然而这种方法中，一种产品对应一个工厂，粒度太小，工厂类过多，维护困难。

**抽象工厂 Abstract Factory**

把有关联关系的产品放在一个抽象工厂内进行生产，派生类重写接口后实例化工厂，用于生产产品。

## 