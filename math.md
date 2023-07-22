# Math

## 向量

### 向量点乘：（内积）

**点乘（Dot Product）**的结果是**点积**，又称**数量积**或**标量积**（Scalar Product）。

![image-20230722175119531](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001283.png)

![image-20230722175150923](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001284.png)

![image-20230722175257909](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001285.png)

### 向量叉乘：（外积）

**叉乘（Cross Product）**又称**向量积**（Vector Product）。

![image-20230722175354615](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001286.png)

![image-20230722175439303](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001287.png)

![image-20230722181532547](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001288.png)

![image-20230722181553851](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001289.png)



### 三角形面积

平行四边形的一半

![image-20230722184247570](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001290.png)

![image-20230722184341392](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001291.png)

![image-20230722184411377](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001292.png)

### 判断点与三角形的关系



#### **Barycentric Coordinates（重心坐标法）**：

Barycentric Coordinates 是一种基于三角形内点在三个顶点上的权重的表示方法。对于三角形ABC，点P在三角形内当且仅当点P的三个重心坐标(alpha, beta, gamma)都在[0, 1]范围内。

```c++
#include <iostream>
using namespace std;

struct Point {
    double x, y;
};

#include <cmath>
bool isAlmostEqual(double a, double b, double epsilon = 1e-10) {
    return std::abs(a - b) < epsilon;
}
double calculateArea(Point A, Point B, Point C) {
    return abs((B.x - A.x) * (C.y - A.y) - (B.y - A.y) * (C.x - A.x)) / 2.0;
}

bool isInsideTriangle(Point A, Point B, Point C, Point P) {
    double totalArea = calculateArea(A, B, C);
    double area1 = calculateArea(P, B, C);
    double area2 = calculateArea(A, P, C);
    double area3 = calculateArea(A, B, P);
    return isAlmostEqual(totalArea, area1 + area2 + area3);
}

int main() {
    Point A = {0.0, 0.0};
    Point B = {1.0, 0.0};
    Point C = {0.5, 1.0};
    Point P = {0.3, 0.3};
    
    if (isInsideTriangle(A, B, C, P)) {
        cout << "Point P is inside the triangle ABC." << endl;
    } else {
        cout << "Point P is outside the triangle ABC." << endl;
    }
    
    return 0;
}
```

在C++中，由于浮点数的特点，我们不能简单地使用"=="运算符来判断浮点数的相等性。这是因为**浮点数存在舍入误差**，可能会导致精度问题。 以下是一种常用的方法，可以在一定误差范围内比较两个浮点数的相等性：



```C++
#include <cmath>
bool isAlmostEqual(double a, double b, double epsilon = 1e-10) {
    return std::abs(a - b) < epsilon;
}
```

#### **Cross Product（叉乘法）**：

 对于三角形ABC，点P在三角形内当且仅当点P与三角形的三个边AB, BC, CA的叉乘方向相同（都在三角形的同一侧）。这个方法需要计算向量叉乘，可以用来判断点在平面上的任意形状是否在三角形内。

推广到多边形同样适用；

![image-20230722200024765](https://gitee.com/juzihhu/image_bed/raw/master/img/202307222001293.png)

```c++
#include <iostream>

struct Point {
    double x, y;
};

// Function to calculate the cross product of two 2D vectors
double crossProduct(const Point& a, const Point& b) {
    return a.x * b.y - a.y * b.x;
}

// Function to check if point P lies inside the triangle formed by A, B, and C
bool isInsideTriangle(const Point& P, const Point& A, const Point& B, const Point& C) {
    Point AB = {B.x - A.x, B.y - A.y};
    Point BC = {C.x - B.x, C.y - B.y};
    Point CA = {A.x - C.x, A.y - C.y};

    Point AP = {P.x - A.x, P.y - A.y};
    Point BP = {P.x - B.x, P.y - B.y};
    Point CP = {P.x - C.x, P.y - C.y};

    // Calculate the z components of the cross products
    double crossAB = crossProduct(AB, AP);
    double crossBC = crossProduct(BC, BP);
    double crossCA = crossProduct(CA, CP);

    // If all cross products have the same sign (positive or negative), point P is inside the triangle
    return ((crossAB >= 0 && crossBC >= 0 && crossCA >= 0) || (crossAB <= 0 && crossBC <= 0 && crossCA <= 0));
}

int main() {
    Point P, A, B, C;
    std::cout << "Enter coordinates of point P (x y): ";
    std::cin >> P.x >> P.y;
    std::cout << "Enter coordinates of triangle vertices A, B, and C (x y): ";
    std::cin >> A.x >> A.y >> B.x >> B.y >> C.x >> C.y;

    if (isInsideTriangle(P, A, B, C)) {
        std::cout << "Point P is inside the triangle formed by A, B, and C." << std::endl;
    } else {
        std::cout << "Point P is outside the triangle formed by A, B, and C." << std::endl;
    }

    return 0;
}
```

