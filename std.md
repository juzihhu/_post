---
title: std使用
date: 2022-03-14 21:48:28
tags: std
categories: c++
cover: https://w.wallhaven.cc/full/zy/wallhaven-zyxvqy.jpg
---

用std:: 而不是 using namsespace std

**原因：全局命名空间中的命名冲突**

在C++中，使用std::前缀代码是一种良好的编程习惯，尤其是在大型代码库中。这是为了避免全局命名空间中的命名冲突，这些命名来自于标准库、其他库或用户代码。

举个例子：

```
Copy code#include <iostream>

int main()
{
    std::string my_string = "Hello World!";
    std::cout << my_string << std::endl;
    return 0;
}
```

在上述代码中，我们使用了标准库中的iostream头文件和string类。如果我们不使用std::前缀，而是使用using namespace std;，则将污染全局命名空间，这样可能会导致命名冲突，例如：

```cpp
Copy code#include <iostream>
using namespace std;

int main()
{
    string my_string = "Hello World!";
    cout << my_string << endl;
    return 0;
}
```

在这段代码中，我们将string类放在全局命名空间中。当我们使用cout时，可能会发生命名冲突，因为有可能存在其他库或用户定义的函数或变量also被命名为cout。而使用std::前缀，我们可以确保我们引用的是标准库的cout函数。

这段代码运行似乎没有发生冲突，这是因为在这个示例程序中，使用using namespace std;不会导致与其他库或用户定义名称之间的冲突。然而，在涉及多个库或多个代码文件的大型项目中，就会出现潜在的命名冲突。如果在一个库和另一个库中使用了相同的函数或变量名称，则在使用using namespace语句时可能会出现名称冲突。因此，在实际开发程序时，为了防止名称冲突，建议使用std::前缀。

因此，为避免命名冲突，最好使用std::前缀。

**给出命名冲突的案例**

假设我们有两个头文件foo.h和bar.h，它们都定义了一个名为my_var的变量：

foo.h:

```cpp
Copy code#pragma once
int my_var = 5;
```

bar.h:

```cpp
Copy code#pragma once
int my_var = 10;
```

如果在我们的程序中同时包含这两个头文件，那么这就会导致命名冲突。请考虑下面的代码：

```cpp
Copy code#include "foo.h"
#include "bar.h"
#include <iostream>

int main()
{
    std::cout << my_var << std::endl;
    return 0;
}
```

这个程序将打印出什么呢？它将会显示一个编译器错误，提示说my_var标识符在全局作用域中已定义，这是因为我们同时包含了foo.h和bar.h，所以my_var在全局作用域中出现两次。如果我们把foo.h和bar.h修改为使用命名空间，则解决命名冲突的问题。

**下面是使用命名空间解决上述问题的代码：**

foo.h:

```cpp
Copy code#pragma once
namespace Foo {
    int my_var = 5;
}
```

bar.h:

```cpp
Copy code#pragma once
namespace Bar {
    int my_var = 10;
}
```

main.cpp:

```cpp
Copy code#include "foo.h"
#include "bar.h"
#include <iostream>

int main()
{
    std::cout << Foo::my_var << std::endl; // 输出5
    std::cout << Bar::my_var << std::endl; // 输出10
    return 0;
}
```

现在，我们将my_var放在不同的命名空间中，以避免全局作用域中的名称冲突。在main函数中，我们需要使用命名空间前缀来访问变量。现在，编译器将能够正确地编译我们的代码，而不会提示my_var在全局作用域中已定义的错误。