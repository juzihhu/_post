---
title: 自定义排序规则
date: 2023-03-14 21:48:28
tags: 

- c++
- 自定义排序规则
  categories: c++
  cover: https://w.wallhaven.cc/full/zy/wallhaven-zygeko.jpg

---

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
class Example {
public:
    //利用仿函数自定义排序规则
    bool operator()(const std::string& a, const std::string& b){
        return a.size()<b.size();
    }  

};

int main() {
    std::vector<std::string> vec = {"apple", "banana", "cherry", "date"};
    // bool operator()(const std::string& a, const std::string& b){
    std:: sort(vec.begin(),vec.end(),Example());
    f or (const auto& s : vec) {
        std::cout << s << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

```cpp
// 重写排序方法自定义排序规则
bool sortByLength(const std::string& a, const std::string& b) {
    return a.size() > b.size();
}
int main() {
    std::sort(vec.begin(), vec.end(),sortByLength);
}
```

```cpp
// 重载运算符<
bool operator<(const std::string& a, const std::string& b){
    return a.size() < b.size(); //按照字符串长度从小到大排序
}
int main() {
    std::vector<std::string> vec = {"apple", "banana", "cherry", "date"};
    std:: sort(vec.begin(),vec.end());
    for (const auto& s : vec) {
        std::cout << s << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#include <string>
class Example {
public:
    static bool sortByLength(const std::string& a, const std::string& b) {
        return a.size() > b.size();
    }
};
int main() {
    std::vector<std::string> vec = {"appl", "banana", "cherry", "date"};
    Example obj;
    std::sort(vec.begin(), vec.end(), obj.sortByLength);
    for (const auto& s : vec) {
        std::cout << s << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

**重定义map内部的Compare函数，按照键字符串长度大小进行排序**

```cpp
#include <map>
#include <string>
#include <iostream>
using namespace std;
// 自己编写的Compare，实现按照字符串长度进行排序
struct CmpByKeyLength {  
  bool operator()(const string& k1, const string& k2) {  
    return k1.length() < k2.length();  
  }  
};  
int main(){
    map<string, int, CmpByKeyLength > mapStudent;  //这里注意要换成自己定义的compare
    mapStudent["LiMin"]=90;
    mapStudent["ZiLinMi"]=72;
    mapStudent["BoB"]=79;
    map<string, int>::iterator iter=mapStudent.begin();
    for(iter=mapStudent.begin();iter!=mapStudent.end();iter++){
        cout<<iter->first<<" "<<iter->second<<endl;
    }
    return 0;
}
```
