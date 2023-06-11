---
title: const成员函数
date: 2022-03-14 21:48:28
tags: const
categories: c++
cover: https://w.wallhaven.cc/full/5g/wallhaven-5gzzy1.jpg

---

## **const成员函数**

const成员函数被声明为const，表明这个函数是**不修改调用对象的状态的。这意味着在该成员函数内部，无法修改调用对象**，包括不修改成员变量的值，也无法调用其他非const成员函数，这是因为非const成员函数可能会更改对象的状态。

## **非const成员函数**

非const成员函数在没有const修饰符的情况下声明。这种成员函数可以更改调用对象的状态，包括修改其成员变量的值，并调用其他非const成员函数。

## **应用举例**

当我们在类中重载比较运算符时，通常会使用成员函数来实现。在成员函数中，我们需要将比较运算符重载为类的一个成员函数，其定义形式为：

```cpp
bool operator< (const MyClass& other) const;
```

其中，`MyClass`是我们需要重载比较运算符的类，`other`是用于比较的另一个对象，而`const`关键字表示该成员函数不会修改对象的状态。

使用`const`关键字的原因是，如果不加上`const`关键字，那么在使用该成员函数时，如果对象是一个常量对象，那么该函数就不能被调用，因为常量对象不能调用非常量成员函数。而在重载比较运算符时，我们往往希望能够比较常量对象，因此需要将该函数声明为常量成员函数。

下面是一个`Person`类的例子，其中使用成员函数重载比较运算符：

```cpp
Person {
public:
    Person(std::string name, int age) : name_(name), age_(age) {}
    bool operator<(const Person& p) const {
        return age_ < p.age_;
    }
private:
    std::string name_;
    int age_;
};
```

在这个例子中，我们重载了小于运算符`<`，并定义了一个以`const`关键字结尾的成员函数。该成员函数接受一个`Person`对象作为参数，并返回一个`bool`类型的值，表示当前对象是否小于参数对象。在这个例子中，我们使用对象的年龄进行比较，如果当前对象的年龄小于参数对象的年龄，则返回`true`，否则返回`false`。

我们可以通过下面的代码来使用上述定义的`Person`类：

```cpp
main() {
    Person p1("Alice", 30);
    Person p2("Bob", 20);
    if (p1 < p2) {
        std::cout << "Alice is younger than Bob" << std::endl;
    } else {
        std::cout << "Bob is younger than Alice" << std::endl;
    }
    return 0;
}
```

在这个例子中，我们定义了两个`Person`对象`p1`和`p2`，并使用重载的小于运算符进行比较。由于`p1`的年龄大于`p2`的年龄，因此`p1 < p2`返回`false`，程序会输出`Bob is younger than Alice`。

## **在调用常量成员函数时，编译器会自动将非常量对象转换为const对象**

常量成员函数声明的形式为在成员函数后面加上const关键字，以示该成员函数不会修改对象的状态。例如：

```cpp
MyClass {
public:
    void myFunction() const;
};
```

现在，假设我们有一个非常量对象obj，可以在这个对象上调用常量成员函数myFunction()，例如：

```cpp
MyClass obj;
obj.myFunction();
```

这个调用虽然看起来违反了“常量成员函数只能被常量对象调用”的规则，但上述代码其实是可以被编译器接受的。这是因为，在调用常量成员函数时，编译器会自动将非常量对象转换为const对象，以避免函数内部更改对象的状态。因此，上述代码可以看做等同于：

```cpp
MyClass obj;
obj.myFunction();
```

这个例子说明了，非常量对象调用常量成员函数时，编译器会自动将对象转换为常量对象，以防止成员函数修改对象的状态，从而保证对象在调用常量成员函数期间保持不变。
