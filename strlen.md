---
title: strlen()函数实现
date: 2023-03-14 21:48:28
tags: strlen()
categories: c++
cover: https://w.wallhaven.cc/full/kx/wallhaven-kxz8z6.jpg
---
c++ strlen()函数，实现源码

下面是C++中的strlen()函数的源码：

```cpp
Copy code#include <iostream>
using namespace std;
int strlen(char* str) {
   int len = 0;
   while (*str != '\0') {
      len++;
      str++;
   }
   return len;
}
int main() {
   char str[] = "Hello, World!";
   int length = strlen(str);
   cout << "Length of string: " << length << endl;
   return 0;
}
```

这个函数使用指针迭代的方法来计算给定字符串的长度。该函数使用while循环来遍历字符串，直到遇到空字符'\0'，然后返回字符串的长度，即字符数。

1. 使用指针和位运算的版本

```cpp
Copy code#include <iostream>
using namespace std;

int strlen(const char* str) {
    const char* c = str;  // 使用const确保不修改传递给函数的字符串
    unsigned int* p = (unsigned int*) c;  // 将字符指针转换成一个4字节的无符号整数指针
    unsigned int v = 0;
    const unsigned int himagic = 0x80808080L;
    const unsigned int lomagic = 0x01010101L;
    
    // 每次读取4个字符，检查是否存在空字符
    while (1) {
        v = *p++;
        if(((v - lomagic) & himagic) != 0) {
            const char* cp = (const char*) (p - 1);
            if (cp[0] == 0) {
                return cp - str;
            }
            if (cp[1] == 0) {
                return cp - str + 1;
            }
            if (cp[2] == 0) {
                return cp - str + 2;
            }
            if (cp[3] == 0) {
                return cp - str + 3;
            }
        }
    }
}

int main() {
    char str[] = "Hello, World!";
    int length = strlen(str);
    cout << "Length of string: " << length << endl;
    return 0;
}
```

这个实现版本基于一个简单的假设，即每个字符都是一个字节，使用32位无符号整数来测试4个字符的空字符(0x00)。每次将字符指针转换为4字节的整数指针，如果第2位比第1位先到达空字符，那么就已经找到了空字符。如果没有找到，就重复这个过程。

1. 使用递归的版本

```cpp
Copy code#include <iostream>
using namespace std;

int strlen_recursive(const char* str, int count = 0) {
    if (str[0] == '\0') {
        return count;
    }
    return strlen_recursive(str + 1, count + 1);
}

int main() {
    char str[] = "Hello, World!";
    int length = strlen_recursive(str);
    cout << "Length of string: " << length << endl;
    return 0;
}
```

这个实现版本使用递归的方式计算字符串的长度。每次检查第一个字符是不是空字符('\0')。如果是，就返回计数器值，否则递归调用该函数，将指针向前移动一位，计数器加1，并尝试下一个字符。直到找到空字符，递归终止，并返回计数器的最终值。

**使用位运算和指针操作计算字符串长度的 C++ 代码，理解这段代码需要掌握以下几个知识点：**

1. 指针的基础知识：指针是一个变量，用来存储内存地址，指向的是内存中某个特定类型的变量。
2. 推断类型转换的方法：(type)expression中，expression是一个表达式，type是需要转换的类型，但是这种方法在某些情况下可能会导致问题。
3. 位运算的基础知识：位运算是对二进制数的位进行操作的运算，常见的位运算有按位与、按位或、按位异或等。

现在来解释代码怎么样实现计算字符串长度。代码中首先定义了一个常量字符指针"c"，指向所传递的字符串 "str"。为了保证不会修改传递给函数的字符串，将指针定义为 const 类型。接下来定义了一个无符号整数指针 "p" 指向 "c" 指针所指向的字符变量地址。由于每次读取 4 个字符，为了提高读取速度，使用一个无符号整数指针可以缩短访问数组的时间。

然后定义了两个无符号整数变量：himagic 和 lomagic。他们的值是相对于二进制值的，用于帮助在以 32 位的块读取字符串时检查字符串的值是否为 0。这样检查相对比使用一个循环更加有效率。

之后开始一个 while 循环，每次读取 4 个字符。如果这个 4 个字符中的任何一个不是字符串结束的空字符，那么 while 循环将不断执行，直至找到字符串结束字符为止。通过*（p++）操作，能够得到一个无符号整数v，这个整数代表每次读取的四个字符。之后使用位运算操作计算是否位空字符。如果不是，将指针向前移动并再次尝试检查。

如果检测到 null 字符，则将指针退回到上一个有用的位置 "p - 1"，将它转换成 char 类型指针，然后从四个可能的字符位置逐个检查，最后找到字符串结束符，并返回结果。在这个实现中，一个4字节的整型变量比4个字节的字符指针更加高效，这明显提高了函数的性能。

下面是加入详细注释后的代码，以便更好地理解：

```cpp
Copy code#include <iostream>
using namespace std;

// 一个用指针操作计算字符串长度的函数
int strlen(const char* str) {

    // 将传入函数的字符串指针定义为 const 类型
    const char* c = str;

    // 将字符指针转换成一个 4 字节的无符号整数指针
    unsigned int* p = (unsigned int*) c;

    // 初始化变量
    unsigned int v = 0;
    const unsigned int himagic = 0x80808080L;
    const unsigned int lomagic = 0x01010101L;

    // 每次读取 4 个字符，检查是否存在空字符('\0')
    while (1) {

        // 读取指向的 4 个字符
        v = *p++;

        // 使用位运算和减法计算是否存在 '\0' 字符
        if(((v - lomagic) & himagic) != 0) {

            // 找到了字符串结束符
            const char* cp = (const char*) (p - 1);

            // 返回字符串的长度
            if (cp[0] == 0) {
                return cp - str;
            }
            if (cp[1] == 0) {
                return cp - str + 1;
            }
            if (cp[2] == 0) {
                return cp - str + 2;
            }
            if (cp[3] == 0) {
                return cp - str + 3;
            }
        }
    }
}

// 主函数
int main() {
    //定义一个字符串
    char str[] = "Hello, World!";
    // 使用自定义的 strlen() 函数计算字符串长度
    int length = strlen(str);
    // 输出结果
    cout << "Length of string: " << length << endl;
    // 结束程序
    return 0;
}
```

这段代码的实现通过字节指针的加减操作和位运算操作来大大提高了计算字符串长度的效率和速度。需要注意的是，在实际使用中，这种方法不是完全可靠的。因为它基于一些固定的假设来简化字符串的内部表示，而对于不同编译器、不同平台和不同库的字符串实现，可能会出现不同的情况。建议在实际生产环境中使用标准库提供的 strlen() 函数来保证代码的可移植性。

