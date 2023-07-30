---
title: 动态规划
date: 2022-05-10 17:10:52
tags: 动态规划
categories: 算法
cover: /img/night-sky-7733876_960_720.jpg
---

本文简单介绍动态规划思想、模板以及部分例题。

<!-- more -->

- [与贪心算法的区别](#与贪心算法的区别)
- [解题步骤](#解题步骤)
- [LeetCode典型题目](#leetcode典型题目)
  - [使用最小花费爬楼梯](#使用最小花费爬楼梯)
  - [不同路径](#不同路径)
  - [整数拆分](#整数拆分)
  - [不同的二叉搜索树](#不同的二叉搜索树)
  - [0-1背包理论基础](#0-1背包理论基础)
  - [分割等和子集](#分割等和子集)
  - [目标和](#目标和)
  - [一和零](#一和零)
  - [完全背包理论](#完全背包理论)
  - [零钱兑换 II](#零钱兑换-ii)
  - [组合总和 Ⅳ](#组合总和-ⅳ)
  - [零钱兑换](#零钱兑换)
  - [单词拆分](#单词拆分)
  - [多重背包理论](#多重背包理论)
  - [打家劫舍](#打家劫舍)
  - [打家劫舍II](#打家劫舍ii)
  - [打家劫舍 III](#打家劫舍-iii)
  - [买卖股票的最佳时机](#买卖股票的最佳时机)
  - [买卖股票的最佳时机II](#买卖股票的最佳时机ii)
  - [买卖股票的最佳时机III](#买卖股票的最佳时机iii)
  - [买卖股票的最佳时机IV](#买卖股票的最佳时机iv)
  - [最长递增子序列](#最长递增子序列)
  - [718. 最长重复子数组](#718-最长重复子数组)

# 与贪心算法的区别

动态规划，英文：Dynamic Programming，简称DP，如果某一问题有很多重叠子问题，使用动态规划是最有效的。
动态规划中每一个状态一定是由上一个状态推导出来的，  
这一点就区分于贪心，贪心没有状态推导，而是从局部直接选最优的，    

# 解题步骤

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

# LeetCode典型题目

## 使用最小花费爬楼梯

[746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs)  

给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

**示例 1：**

    输入：cost = [10,15,20]
    输出：15
    解释：你将从下标为 1 的台阶开始。
    - 支付 15 ，向上爬两个台阶，到达楼梯顶部。
    总花费为 15 。


​    
    输入：cost = [1,100,1,1,1,100,1,1,100,1]

**示例 2：**

    输出：6
    解释：你将从下标为 0 的台阶开始。
    - 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
    - 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
    - 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
    - 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
    - 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
    - 支付 1 ，向上爬一个台阶，到达楼梯顶部。
    总花费为 6 。

1. 确定dp数组以及下标的含义
  
    dp[i]的定义：到达第i个台阶后继续向上所花费的最少体力为dp[i]

2. 确定递推公式
   可以有两个途径得到dp[i]，一个是dp[i-1] 一个是dp[i-2]。   
   所以dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i];   

3. dp数组如何初始化   
   `vector<int> dp(cost.size());`  
   `dp[0] = cost[0];`  
   `dp[1] = cost[1];`  

4. 确定遍历顺序  
   因为是模拟台阶，而且dp[i]又dp[i-1]dp[i-2]推出，所以是从前到后遍历cost数组就可以了。  
   
   但是稍稍有点难度的动态规划，其遍历顺序并不容易确定下来。   
   
   例如：01背包，都知道两个for循环，一个for遍历物品嵌套一个for遍历背包容量，那么为什么不是一个for遍历背包容量嵌套一个for遍历物品呢？ 以及在使用一维dp数组的时候遍历背包容量为什么要倒序呢

5. 举例推导dp数组   
   拿示例2：cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1] ，来模拟一下dp数组的状态变化，如下：    
   
   1 100 2 3 3 103 4 5 104 6 

```c++
// 版本一
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size());
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.size(); i++) {
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i];
        }
        // 注意最后一步可以理解为不用花费，所以取倒数第一步，第二步的最少值
        return min(dp[cost.size() - 1], dp[cost.size() - 2]);
    }
};
```

还可以优化空间复杂度，因为dp[i]就是由前两位推出来的，那么也不用dp数组了，C++代码如下：

```c++
// 版本二
class Solution {
public:
   int minCostClimbingStairs(vector<int>& cost) {
       int dp0 = cost[0];
       int dp1 = cost[1];
       for (int i = 2; i < cost.size(); i++) {
           int dpi = min(dp0, dp1) + cost[i];
           dp0 = dp1; // 记录一下前两位
           dp1 = dpi;
       }
       return min(dp0, dp1);
   }
};
```

 从题目描述可以看出：要不是第一步不需要花费体力，要不就是第最后一步不需要花费体力，
 如果理解为第一步不需要花费体力

```c++
class Solution {
private:

public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n=cost.size();
        vector<int> dp(1001,-1);
        dp[0]=0;
        dp[1]=0;
        for(int i=2;i<=n;++i){
            dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[n];
    }
};
```

## 不同路径

[62. 不同路径](https://leetcode.cn/problems/unique-paths)

 一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

    输入：m = 3, n = 2
    输出：3
    解释：
    从左上角开始，总共有 3 条路径可以到达右下角。
    1. 向右 -> 向下 -> 向下
    2. 向下 -> 向下 -> 向右
    3. 向下 -> 向右 -> 向下

**思路**

1. 深度优先搜索（时间超限）

```c++
class Solution {
private:
    int dfs(int i, int j, int m, int n) {
        if (i > m || j > n) return 0; // 越界了
        if (i == m && j == n) return 1; // 找到一种方法，相当于找到了叶子节点
        return dfs(i + 1, j, m, n) + dfs(i, j + 1, m, n);
    }
public:
    int uniquePaths(int m, int n) {
        return dfs(1, 1, m, n);
    }
};
```

这棵树的深度其实就是m+n-1（深度按从1开始计算）。

那二叉树的节点个数就是 2^(m + n - 1) - 1。可以理解深搜的算法就是遍历了整个满二叉树（其实没有遍历整个满二叉树，只是近似而已）

所以上面深搜代码的时间复杂度为O(2^(m + n - 1) - 1)，可以看出，这是指数级别的时间复杂度，是非常大的。

2. 动态规划

按照动规五部曲来分析：

- 确定dp数组（dp table）以及下标的含义
  
    dp[i][j] ：表示从（0 ，0）出发，到(i, j) 有dp[i][j]条不同的路径。

- 确定递推公式
  
    想要求dp[i][j]，只能有两个方向来推导出来，即dp[i - 1][j] 和 dp[i][j - 1]。
  
    此时在回顾一下 dp[i - 1][j] 表示啥，是从(0, 0)的位置到(i - 1, j)有几条路径，dp[i][j - 1]同理。
  
    那么很自然，`dp[i][j] = dp[i - 1][j] + dp[i][j - 1]`，因为dp[i][j]只有这两个方向过来。

- dp数组的初始化
  
    如何初始化呢，首先dp[i][0]一定都是1，因为从(0, 0)的位置到(i, 0)的路径只有一条，那么dp[0][j]也同理。
  所以初始化代码为：
    `for (int i = 0; i < m; i++) dp[i][0] = 1;`
    `for (int j = 0; j < n; j++) dp[0][j] = 1;`

- 确定遍历顺序
  
    这里要看一下递归公式dp[i][j] = dp[i - 1][j] + dp[i][j - 1]，dp[i][j]都是从其上方和左方推导而来，那么从左到右一层一层遍历就可以了。
  
    这样就可以保证推导dp[i][j]的时候，dp[i - 1][j] 和 dp[i][j - 1]一定是有数值的。

- 举例推导dp数组

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 0; j < n; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

**优化：** 画一下dp[]推到数组就可以理解了

`dp[i] += dp[i] + dp[i - 1];` 

`dp[i]`可以理解为上一行的`dp[i-1][j]`

`dp[i - 1]`可以理解为`dp[i][j-1]`

滚动数组的理解

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> dp(n);
        for (int i = 0; i < n; i++) dp[i] = 1;
        for (int j = 1; j < m; j++) {
            for (int i = 1; i < n; i++) {
                dp[i] += dp[i - 1];
            }
        }
        return dp[n - 1];
    }
};
```

3. 数论方法

无论怎么走，走到终点都需要 m + n - 2 步。
在这m + n - 2 步中，一定有 m - 1 步是要向下走的，不用管什么时候向下走
那么这就是一个组合问题了。

**求组合的时候，要防止两个int相乘溢出！ 所以不能把算式的分子都算出来，分母都算出来再做除法。**

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        long long numerator = 1; // 分子
        int denominator = m - 1; // 分母
        int count = m - 1;
        int t = m + n - 2;
        while (count--) {
            numerator *= (t--);
            while (denominator != 0 && numerator % denominator == 0) {
                numerator /= denominator;
                denominator--;
            }
        }
        return numerator;
    }
};
```

 **提升**

[63. 不同路径 II](https://leetcode.cn/problems/unique-paths-ii/)
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

```cpp
class Solution {
public:
   int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
       if (obstacleGrid[0][0] == 1)
           return 0;
       vector<int> dp(obstacleGrid[0].size());
       for (int j = 0; j < dp.size(); ++j)
           if (obstacleGrid[0][j] == 1)
               dp[j] = 0;
           else if (j == 0)
               dp[j] = 1;
           else
               dp[j] = dp[j-1];

       for (int i = 1; i < obstacleGrid.size(); ++i)
           for (int j = 0; j < dp.size(); ++j){
               if (obstacleGrid[i][j] == 1)
                   dp[j] = 0;
               else if (j != 0)
                   dp[j] = dp[j] + dp[j-1];
           }
       return dp.back();
   }
};
```

## 整数拆分

[343. 整数拆分](https://leetcode.cn/problems/integer-break/)
给定一个正整数 n ，将其拆分为 k 个 正整数 的和（ k >= 2 ），并使这些整数的乘积最大化。
返回 你可以获得的最大乘积 

**思路：**

1. 数学证明+贪心

```cpp
class Solution {
public:
    long long fun(long long a,int x){
        int result=1;
        while(x>0){
            if(x&1) result*=a;
            x=x>>1;
            a=a*a;
        }
        return result;
    }
    int integerBreak(int n) {
        if(n==2) return 1;
        if(n==3) return 2;
        if(n==4) return 4;
        int num3=0,num2=1;
        long long result=0;
        num3=n/3;
        n=n%3;
        if(n==1){
            --num3;
            num2=4;
        }else if(n==2){
            num2=2;
        }
        result+=fun(3,num3);
        result*=num2;

        return result;

    }
};
```

2. 动态规划

dp[i]：分拆数字i，可以得到的最大乘积为dp[i]。
递推公式：`dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));`

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n + 1);
        dp[2] = 1;
        for (int i = 3; i <= n ; i++) {
            for (int j = 1; j < i - 1; j++) {
                dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
};
```

## 不同的二叉搜索树

[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？     
返回满足题意的二叉搜索树的种数。

![](https://gitee.com/juzihhu/image_bed/raw/master/img/%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%911.png)
![](https://gitee.com/juzihhu/image_bed/raw/master/img/%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%912.png)

来看看n为3的时候，有哪几种情况。

当1为头结点的时候，其右子树有两个节点，看这两个节点的布局，是不是和 n 为2的时候两棵树的布局是一样的啊！

**（注意求不同树的数量，并不用把搜索树都列出来，所以不用关心其具体数值的差异）**

当3为头结点的时候，其左子树有两个节点，看这两个节点的布局，是不是和n为2的时候两棵树的布局也是一样的啊！

当2为头结点的时候，其左右子树都只有一个节点，布局是不是和n为1的时候只有一棵树的布局也是一样的啊！

发现到这里，其实我们就找到了重叠子问题了，其实也就是发现可以通过dp[1] 和 dp[2] 来推导出来dp[3]的某种方式。

思考到这里，这道题目就有眉目了。

dp[3]，就是 元素1为头结点搜索树的数量 + 元素2为头结点搜索树的数量 + 元素3为头结点搜索树的数量

元素1为头结点搜索树的数量 = 右子树有2个元素的搜索树数量 * 左子树有0个元素的搜索树数量

元素2为头结点搜索树的数量 = 右子树有1个元素的搜索树数量 * 左子树有1个元素的搜索树数量

元素3为头结点搜索树的数量 = 右子树有0个元素的搜索树数量 * 左子树有2个元素的搜索树数量

有2个元素的搜索树数量就是dp[2]。

有1个元素的搜索树数量就是dp[1]。

有0个元素的搜索树数量就是dp[0]。

所以dp[3] = dp[2] * dp[0] + dp[1] * dp[1] + dp[0] * dp[2]

**dp[i] ： 1到i为节点组成的二叉搜索树的个数为dp[i]。**
递推公式：`dp[i] += dp[j - 1] * dp[i - j]; ，j-1 `为j为头结点左子树节点数量，i-j 为以j为头结点右子树节点数量

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```

**方法二：数学**

dp[i]的值为卡塔兰数
![](https://gitee.com/juzihhu/image_bed/raw/master/img/equation.svg)
![](https://gitee.com/juzihhu/image_bed/raw/master/img/%E5%8D%A1%E5%A1%94%E5%85%B0%E6%95%B0%E9%80%92%E6%8E%A8%E5%85%AC%E5%BC%8F.png)

```cpp
class Solution {
public:
    int numTrees(int n) {
        long long C = 1;
        for (int i = 0; i < n; ++i) {
            C = C * 2 * (2 * i + 1) / (i + 2);
        }
        return (int)C;
    }
};
```

[卡特兰数介绍](https://zhuanlan.zhihu.com/p/97619085)

## 0-1背包理论基础

有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。
每件物品只能用一次，求解将哪些物品装入背包里物品价值总和最大。

**暴力思想**

每一件物品其实只有两个状态，取或者不取，所以可以使用回溯法搜索出所有的情况，那么时间复杂度就是$o(2^n)$，这里的n表示物品数量。

重量     价值
物品0     1         15
物品1     3         20
物品2     4         30

**二维dp数组01背包**

**首先理解动态规划与贪心的区别在于：动态规划有状态推导**

1. 确定dp数组以及下标的含义
   dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。

2. 确定递推公式
   再回顾一下dp[i][j]的含义：从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。
   
    那么可以有两个方向推出来dp[i][j]:
   
   - 不放物品i：由dp[i - 1][j]推出，即背包容量为j，里面不放物品i的最大价值，此时dp[i][j]就是dp[i - 1][j]。(其实就是当物品i的重量大于背包j的重量时，物品i无法放进背包中，所以被背包内的价值依然和前面相同。)
   
   - 放物品i：由`dp[i - 1][j - weight[i]]`推出，`dp[i - 1][j - weight[i]] `为背包容量为j - weight[i]的时候不放物品i的最大价值，那么`dp[i - 1][j - weight[i]] + value[i] `（物品i的价值），就是背包放物品i得到的最大价值
     
     所以递归公式： `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`

3. dp数组如何初始化
   首先从dp[i][j]的定义出发，如果背包容量j为0的话，即dp[i][0]，无论是选取哪些物品，背包价值总和一定为0。       
   状态转移方程 dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]); 可以看出i 是由 i-1 推导出来，那么i为0的时候就一定要初始化
   ![](https://gitee.com/juzihhu/image_bed/raw/master/img/01%E8%83%8C%E5%8C%85dp%E5%88%9D%E5%A7%8B%E5%8C%96.png)

```cpp
// 初始化 dp
vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
for (int j = weight[0]; j <= bagweight; j++) {
    dp[0][j] = value[0];
}
```

4. 确定遍历顺序
   有两个遍历的维度：物品与背包重量

**先遍历物品更好理解**

```cpp
// weight数组的大小 就是物品个数
for(int i = 1; i < weight.size(); i++) { // 遍历物品
    for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j]; 
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

    }
}
```

先遍历背包，再遍历物品，也是可以的！（注意我这里使用的二维dp数组）

```cpp
// weight数组的大小 就是物品个数
for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}
```

`dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);` 递归公式中可以看出dp[i][j]是靠dp[i-1][j]和dp[i - 1][j - weight[i]]推导出来的。

dp[i-1][j]和dp[i - 1][j - weight[i]] 都在dp[i][j]的左上角方向（包括正上方向）   

![](https://gitee.com/juzihhu/image_bed/raw/master/img/01%E8%83%8C%E5%8C%85dp%E6%8E%A8%E5%AF%BC%E8%BF%87%E7%A8%8B.png)

5. 举例推导结果

![](https://gitee.com/juzihhu/image_bed/raw/master/img/01%E8%83%8C%E5%8C%85dp%E6%8E%A8%E5%AF%BC%E7%BB%93%E6%9E%9C.png)

```cpp
void test_2_wei_bag_problem1() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagweight = 4;

    // 二维数组
    vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));

    // 初始化
    for (int j = weight[0]; j <= bagweight; j++) {
        dp[0][j] = value[0];
    }

    // weight数组的大小 就是物品个数
    for(int i = 1; i < weight.size(); i++) { // 遍历物品
        for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

        }
    }

    cout << dp[weight.size() - 1][bagweight] << endl;
}

int main() {
    test_2_wei_bag_problem1();
}
```

**难点在于如何初始化和遍历顺序上**

**滚动数组实现01背包问题**
在二维的dp推导中可以发现，`dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`
当前的推导来源于推导表格的上一行，故而可以用一维数组实现动态规划（可以先理解**不同路径**那题中的一维数组实现）

在使用二维数组的时候，递推公式：`dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);`

其实可以发现如果把dp[i - 1]那一层拷贝到dp[i]上，表达式完全可以是：`dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);`

与其把dp[i - 1]这一层拷贝到dp[i]上，不如只用一个一维数组了，只用dp[j]（一维数组，也可以理解是一个滚动数组）。

这就是滚动数组的由来，需要满足的条件是上一层可以重复利用，直接拷贝到当前层。

1. 确定dp数组的定义

> dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。

2. 递推公式

> dp[j]可以通过dp[j - weight[i]]推导出来，dp[j - weight[i]]表示容量为j - weight[i]的背包所背的最大价值。

> `dp[j - weight[i]] + value[i] `表示 容量为 j - 物品i重量 的背包 加上 物品i的价值。（也就是容量为j的背包，放入物品i了之后的价值即：dp[j]）

> 此时dp[j]有两个选择，一个是取自己dp[j] 相当于 二维dp数组中的dp[i-1][j]，即不放物品i
> 一个是取dp[j - weight[i]] + value[i]，即放物品i，指定是取最大的，毕竟是求最大价值

> `dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);`

3. 一维dp数组如何初始化
   dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]，那么dp[0]就应该是0，因为背包容量为0所背的物品的最大价值就是0。
   
    那么dp数组除了下标0的位置，初始为0，其他下标应该初始化多少呢？
   
    看一下递归公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
   
    dp数组在推导的时候一定是取价值最大的数，如果题目给的价值都是正整数那么非0下标都初始化为0就可以了。
   
    这样才能让dp数组在递归公式的过程中取的最大的价值，而不是被初始值覆盖了。
   
    那么我假设物品价值都是大于0的，所以dp数组初始化的时候，都初始为0就可以了。

4. 一维dp数组遍历顺序

```cpp
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

这里大家发现和二维dp的写法中，遍历背包的顺序是不一样的！

二维dp遍历的时候，背包容量是从小到大，而一维dp遍历的时候，背包是从大到小。

为什么呢？

倒序遍历是为了保证物品i只被放入一次！。但如果一旦正序遍历了，那么物品0就会被重复加入多次！

举一个例子：物品0的重量weight[0] = 1，价值value[0] = 15

如果正序遍历

dp[1] = dp[1 - weight[0]] + value[0] = 15

dp[2] = dp[2 - weight[0]] + value[0] = 30

此时dp[2]就已经是30了，意味着物品0，被放入了两次，所以不能正序遍历。

为什么倒序遍历，就可以保证物品只放入一次呢？

倒序就是先算dp[2]

dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）

dp[1] = dp[1 - weight[0]] + value[0] = 15

所以从后往前循环，每次取得状态不会和之前取得状态重合，这样每种物品就只取一次了。

那么问题又来了，为什么二维dp数组历的时候不用倒序呢？

因为对于二维dp，dp[i][j]都是通过上一层即dp[i - 1][j]计算而来，本层的dp[i][j]并不会被覆盖！

![](https://gitee.com/juzihhu/image_bed/raw/master/img/01%E8%83%8C%E5%8C%85%E4%B8%80%E7%BB%B4dp%E6%8E%A8%E5%AF%BC.png)

```cpp
void test_1_wei_bag_problem() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 初始化
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}

int main() {
    test_1_wei_bag_problem();
}
```

## 分割等和子集

[416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意: 每个数组中的元素不会超过 100 数组的大小不会超过 200

示例 1: 输入: [1, 5, 11, 5] 输出: true 解释: 数组可以分割成 [1, 5, 5] 和 [11].

示例 2: 输入: [1, 2, 3, 5] 输出: false 解释: 数组不能分割成两个元素和相等的子集.

**思路：**

1. 回溯求相应的子集是否存在
2. 01背包的角度解决问题

确定dp数组以及下标的含义

> 01背包中，dp[j] 表示： 容量为j的背包，所背的物品价值可以最大为dp[j]。
> 套到本题，dp[j]表示 背包总容量是j，最大可以凑成j的子集总和为dp[j]。

确定递推公式

> `dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);`

dp数组如何初始化

> // 题目中说：每个数组中的元素不会超过 100，数组的大小不会超过 200
> // 总和不会大于20000，背包最大只需要其中一半，所以10001大小就可以了
> `vector<int> dp(10001, 0);`

确定遍历顺序

```cpp
// 开始 01背包
for(int i = 0; i < nums.size(); i++) {
    for(int j = target; j >= nums[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
        dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
    }
}
```

 举例推导dp数组

```cpp
class Solution {
public:
   bool canPartition(vector<int>& nums) {
       int sum = 0;

       // dp[i]中的i表示背包内总和
       // 题目中说：每个数组中的元素不会超过 100，数组的大小不会超过 200
       // 总和不会大于20000，背包最大只需要其中一半，所以10001大小就可以了
       vector<int> dp(10001, 0);
       for (int i = 0; i < nums.size(); i++) {
           sum += nums[i];
       }
       if (sum % 2 == 1) return false;
       int target = sum / 2;

       // 开始 01背包
       for(int i = 0; i < nums.size(); i++) {
           for(int j = target; j >= nums[i]; j--) { // 每一个元素一定是不可重复放入，所以从大到小遍历
               dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
           }
       }
       // 集合中的元素正好可以凑成总和target
       if (dp[target] == target) return true;
       return false;
   }
};
```

## 目标和

[494. 目标和](https://leetcode-cn.com/problems/target-sum/)
 给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

示例：

输入：nums: [1, 1, 1, 1, 1], S: 3
输出：5

解释：
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。

**思路**
本题要如何使表达式结果为target，

既然为target，那么就一定有 left组合 - right组合 = target。

left + right等于sum，而sum是固定的。

公式来了， left - (sum - left) = target -> left = (target + sum)/2 。

target是固定的，sum是固定的，left就可以求出来。

**从而可以转化为01背包问题**

```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) sum += nums[i];
        if (abs(S) > sum) return 0; // 此时没有方案
        if ((S + sum) % 2 == 1) return 0; // 此时没有方案
        int bagSize = (S + sum) / 2;
        vector<int> dp(bagSize + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = bagSize; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
};
```

## 一和零

[474. 一和零](https://leetcode.cn/problems/ones-and-zeroes)

给你一个二进制字符串数组 strs 和两个整数 m 和 n 。
请你找出并返回 strs 的最大子集的长度，该子集中 最多 有 m 个 0 和 n 个 1 。
如果 x 的所有元素也是 y 的元素，集合 x 是集合 y 的 子集 。

    输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
    输出：4
    解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
    其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 
    不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。

1. 确定dp数组（dp table）以及下标的含义
   dp[i][j]：最多有i个0和j个1的strs的最大子集的大小为dp[i][j]。

2. 确定递推公式
   dp[i][j] 可以由前一个strs里的字符串推导出来，strs里的字符串有zeroNum个0，oneNum个1。
   dp[i][j] 就可以是 dp[i - zeroNum][j - oneNum] + 1
   然后我们在遍历的过程中，取dp[i][j]的最大值。
   所以递推公式：dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);

这就是一个典型的01背包！ 只不过物品的重量有了两个维度而已。

3. dp数组如何初始化
4. 确定遍历顺序
5. 举例推导dp数组

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int> (n + 1, 0)); // 默认初始化0
        for (string str : strs) { // 遍历物品
            int oneNum = 0, zeroNum = 0;
            for (char c : str) {
                if (c == '0') zeroNum++;
                else oneNum++;
            }
            for (int i = m; i >= zeroNum; i--) { // 遍历背包容量且从后向前遍历！
                for (int j = n; j >= oneNum; j--) {
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

## 完全背包理论

 有N件物品和一个最多能背重量为W的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。
 每件物品都有无限个（也就是可以放入背包多次），求解将哪些物品装入背包里物品价值总和最大。

 首先在回顾一下01背包的核心代码

```cpp
for(int i = 0; i < weight.size(); i++) { // 遍历物品
   for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
       dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
   }
}
```

我们知道01背包内嵌的循环是从大到小遍历，为了保证每个物品仅被添加一次。

而完全背包的物品是可以添加多次的，所以要从小到大去遍历，即：

```cpp
// 先遍历物品，再遍历背包
for(int i = 0; i < weight.size(); i++) { // 遍历物品
    for(int j = weight[i]; j <= bagWeight ; j++) { // 遍历背包容量
        dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

    }
}
```

在完全背包中，对于一维dp数组来说，其实两个for循环嵌套顺序同样无所谓！

因为dp[j] 是根据 下标j之前所对应的dp[j]计算出来的。 只要保证下标j之前的dp[j]都是经过计算的就可以了。

```cpp
// 先遍历背包，再遍历物品
for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        if (j - weight[i] >= 0) dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
    }
    cout << endl;
}
```

## 零钱兑换 II

[518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-2) 
给你一个整数数组 coins 表示不同面额的硬币，另给一个整数 amount 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 0 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

    输入：amount = 5, coins = [1, 2, 5]
    输出：4
    解释：有四种方式可以凑成总金额：
    5=5
    5=2+2+1
    5=2+1+1+1
    5=1+1+1+1+1

**思路：**

dp[j]：凑成总金额j的货币组合数为dp[j]
dp[j] （考虑coins[i]的组合总和） 就是所有的dp[j - coins[i]]（不考虑coins[i]）相加。
所以递推公式：`dp[j] += dp[j - coins[i]];`  

首先dp[0]一定要为1，dp[0] = 1是 递归公式的基础。
从dp[i]的含义上来讲就是，凑成总金额0的货币组合数为1。
下标非0的dp[j]初始化为0，这样累计加dp[j - coins[i]]的时候才不会影响真正的dp[j]

**因为纯完全背包求得是能否凑成总和，和凑成总和的元素有没有顺序没关系，即：有顺序也行，没有顺序也行！**
而本题要求凑成总和的组合数，元素之间要求没有顺序。
所以纯完全背包是能凑成总和就行，不用管怎么凑的。
本题是求凑出来的方案个数，且每个方案个数是为组合数。
那么本题，两个for循环的先后顺序可就有说法了。
我们先来看 外层for循环遍历物品（钱币），内层for遍历背包（金钱总额）的情况。

```cpp
for (int i = 0; i < coins.size(); i++) { // 遍历物品
    for (int j = coins[i]; j <= amount; j++) { // 遍历背包容量
        dp[j] += dp[j - coins[i]];
    }
}
```

假设：coins[0] = 1，coins[1] = 5。

那么就是先把1加入计算，然后再把5加入计算，得到的方法数量只有{1, 5}这种情况。而不会出现{5, 1}的情况。

**所以这种遍历顺序中dp[j]里计算的是组合数！**

```cpp
for (int j = 0; j <= amount; j++) { // 遍历背包容量
    for (int i = 0; i < coins.size(); i++) { // 遍历物品  
        if (j - coins[i] >= 0) dp[j] += dp[j - coins[i]];
    }
}
```

**背包容量的每一个值，都是经过 1 和 5 的计算，包含了{1, 5} 和 {5, 1}两种情况。**

此时dp[j]里算出来的就是排列数！

![](https://gitee.com/juzihhu/image_bed/raw/master/img/%E5%AE%8C%E5%85%A8%E8%83%8C%E5%8C%85%E9%9B%B6%E9%92%B1%E5%85%91%E6%8D%A2dp%E6%8E%A8%E5%AF%BC.png)

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < coins.size(); i++) { // 遍历物品
            for (int j = coins[i]; j <= amount; j++) { // 遍历背包
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```

## 组合总和 Ⅳ

[377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv)

给你一个由 不同 整数组成的数组 nums ，和一个目标整数 target 。请你从 nums 中找出并返回总和为 target 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

    输入：nums = [1,2,3], target = 4
    输出：7
    解释：
    所有可能的组合为：
    (1, 1, 1, 1)
    (1, 1, 2)
    (1, 2, 1)
    (1, 3)
    (2, 1, 1)
    (2, 2)

**本质为求背包排列数，故而应该先遍历背包容量**

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for (int i = 0; i <= target; i++) { // 遍历背包
            for (int j = 0; j < nums.size(); j++) { // 遍历物品
                if (i - nums[j] >= 0 && dp[i] < INT_MAX - dp[i - nums[j]]) {
                    dp[i] += dp[i - nums[j]];
                }
            }
        }
        return dp[target];
    }
};
```

## 零钱兑换

[322. 零钱兑换](https://leetcode.cn/problems/coin-change)

给你一个整数数组 coins ，表示不同面额的硬币；以及一个整数 amount ，表示总金额。

计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 -1 。

你可以认为每种硬币的数量是无限的。

```cpp
// 版本一
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i < coins.size(); i++) { // 遍历物品
            for (int j = coins[i]; j <= amount; j++) { // 遍历背包
                if (dp[j - coins[i]] != INT_MAX) { // 如果dp[j - coins[i]]是初始值则跳过
                    dp[j] = min(dp[j - coins[i]] + 1, dp[j]);
                }
            }
        }
        if (dp[amount] == INT_MAX) return -1;
        return dp[amount];
    }
};
```

**注意**

` if (dp[j - coins[i]] != INT_MAX) { // 如果dp[j - coins[i]]是初始值则跳过`
对于无法实现的dp[j]需要跳过

相似题：

[279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

## 单词拆分

[139. 单词拆分](https://leetcode.cn/problems/word-break)

给你一个字符串 s 和一个字符串列表 wordDict 作为字典。请你判断是否可以利用字典中出现的单词拼接出 s 。

注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。

    输入: s = "leetcode", wordDict = ["leet", "code"]
    输出: true
    解释: 返回 true 因为 "leetcode" 可以由 "leet" 和 "code" 拼接成。

**思路：**

1. 常规回溯法
  
   ```cpp
   class Solution {
   private:
    bool backtracking (const string& s, const unordered_set<string>& wordSet, int startIndex) {
        if (startIndex >= s.size()) {
            return true;
        }
        for (int i = startIndex; i < s.size(); i++) {
            string word = s.substr(startIndex, i - startIndex + 1);
            if (wordSet.find(word) != wordSet.end() && backtracking(s, wordSet, i + 1)) {
                return true;
            }
        }
        return false;
    }
   public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        return backtracking(s, wordSet, 0);
    }
   };
   ```

改进回溯：

```cpp
class Solution {
private:
    bool backtracking (const string& s,
            const unordered_set<string>& wordSet,
            vector<bool>& memory,
            int startIndex) {
        if (startIndex >= s.size()) {
            return true;
        }
        // 如果memory[startIndex]不是初始值了，直接使用memory[startIndex]的结果
        if (!memory[startIndex]) return memory[startIndex];
        for (int i = startIndex; i < s.size(); i++) {
            string word = s.substr(startIndex, i - startIndex + 1);
            if (wordSet.find(word) != wordSet.end() && backtracking(s, wordSet, memory, i + 1)) {
                return true;
            }
        }
        memory[startIndex] = false; // 记录以startIndex开始的子串是不可以被拆分的
        return false;
    }
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
        vector<bool> memory(s.size(), 1); // -1 表示初始化状态
        return backtracking(s, wordSet, memory, 0);
    }
};
```

 **动态规划**

 dp[i] : 字符串长度为i的话，dp[i]为true，表示可以拆分为一个或多个在字典中出现的单词

 **如果确定dp[j] 是true，且 [j, i] 这个区间的子串出现在字典里，那么dp[i]一定是true。（j < i ）**
 所以递推公式是 if([j, i] 这个区间的子串出现在字典里 && dp[j]是true) 那么 dp[i] = true

 从递归公式中可以看出，**dp[i] 的状态依靠 dp[j]是否为true，那么dp[0]就是递归的根基，dp[0]一定要为true**，否则递归下去后面都都是false了

 下标非0的dp[i]初始化为false，只要没有被覆盖说明都是不可拆分为一个或多个在字典中出现的单词。

 题目中说是拆分为一个或多个在字典中出现的单词，所以这是完全背包
 **如果求组合数就是外层for循环遍历物品，内层for遍历背包。
 如果求排列数就是外层for遍历背包，内层for循环遍历物品。**
 本题最终要求的是是否都出现过，所以对出现单词集合里的元素是组合还是排列，并不在意！

```cpp
class Solution {
public:
   bool wordBreak(string s, vector<string>& wordDict) {
       unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
       vector<bool> dp(s.size() + 1, false);
       dp[0] = true;
       for (int i = 1; i <= s.size(); i++) {   // 遍历背包
           for (int j = 0; j < i; j++) {       // 遍历物品
               string word = s.substr(j, i - j); //substr(起始位置，截取的个数)
               if (wordSet.find(word) != wordSet.end() && dp[j]) {
                   dp[i] = true;
               }
           }
       }
       return dp[s.size()];
   }
};
```

 **错误思想：**
 以字典中单词为物品

```cpp
class Solution {
public:
   bool wordBreak(string s, vector<string>& wordDict) {
       string dp="";
       while(dp.size()<=s.size()){
           for(int j=0;j<wordDict.size();++j){
               dp=(dp==s)?dp:dp+wordDict[j];
               if(dp==s) return true;
           }
       }
       return false;
   }
};
```

## 多重背包理论

有N种物品和一个容量为V 的背包。第i种物品最多有Mi件可用，每件耗费的空间是Ci ，价值是Wi 
求解将哪些物品装入背包可使这些物品的耗费的空间 总和不超过背包容量，且价值总和最大

**每件物品最多有Mi件可用，把Mi件摊开，其实就是一个01背包问题了**

```cpp
void test_multi_pack() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    vector<int> nums = {2, 3, 2};
    int bagWeight = 10;
    for (int i = 0; i < nums.size(); i++) {
        while (nums[i] > 1) { // nums[i]保留到1，把其他物品都展开
            weight.push_back(weight[i]);
            value.push_back(value[i]);
            nums[i]--;
        }
    }

    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
        for (int j = 0; j <= bagWeight; j++) {
            cout << dp[j] << " ";
        }
        cout << endl;
    }
    cout << dp[bagWeight] << endl;

}
int main() {
    test_multi_pack();
}
```

## 打家劫舍

[198. 打家劫舍](https://leetcode.cn/problems/house-robber)

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，
如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

    输入：[1,2,3,1]
    输出：4
    解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
        偷窃到的最高金额 = 1 + 3 = 4 。

dp[i]：考虑下标i（包括i）以内的房屋，最多可以偷窃的金额为dp[i]。

决定dp[i]的因素就是第i房间偷还是不偷。

如果偷第i房间，那么dp[i] = dp[i - 2] + nums[i] ，即：第i-1房一定是不考虑的，找出 下标i-2（包括i-2）以内的房屋，最多可以偷窃的金额为dp[i-2] 加上第i房间偷到的钱。

如果不偷第i房间，那么dp[i] = dp[i - 1]，即考虑i-1房，（注意这里是考虑，并不是一定要偷i-1房，这是很多同学容易混淆的点）

然后dp[i]取最大值，即dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.size() - 1];
    }
};
```

## 打家劫舍II

 你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，能够偷窃到的最高金额。

    输入：nums = [2,3,2]
    输出：3
    解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。

情况一：考虑不包含首尾元素
情况二：考虑包含首元素，不包含尾元素
情况三：考虑包含尾元素，不包含首元素

```cpp
// 注意注释中的情况二情况三，以及把198.打家劫舍的代码抽离出来了
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        int result1 = robRange(nums, 0, nums.size() - 2); // 情况二
        int result2 = robRange(nums, 1, nums.size() - 1); // 情况三
        return max(result1, result2);
    }
    // 198.打家劫舍的逻辑
    int robRange(vector<int>& nums, int start, int end) {
        if (end == start) return nums[start];
        vector<int> dp(nums.size());
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start], nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
};
```

## 打家劫舍 III

[打家劫舍 III](https://leetcode.cn/problems/house-robber-iii)
小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 root 。

除了 root 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。

给定二叉树的 root 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。

```cpp
class Solution {
public:
    int rob(TreeNode* root) {
        vector<int> result = robTree(root);
        return max(result[0], result[1]);
    }
    // 长度为2的数组，0：不偷，1：偷
    vector<int> robTree(TreeNode* cur) {
        if (cur == NULL) return vector<int>{0, 0};
        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        // 偷cur
        int val1 = cur->val + left[0] + right[0];
        // 不偷cur
        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2, val1};
    }
};
```

## 买卖股票的最佳时机

[买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

    示例 1：
    输入：[7,1,5,3,6,4]
    输出：5
    解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。

dp[i][0] 表示第i天持有股票所得最多现金 ，那么加入第i天买入股票现金就是 -prices[i]， 这是一个负数。

dp[i][1] 表示第i天不持有股票所得最多现金

注意这里说的是“持有”，“持有”不代表就是当天“买入”！也有可能是昨天就买入了，今天保持持有的状态

如果第i天持有股票即dp[i][0]， 那么可以由两个状态推出来

- 第i-1天就持有股票，那么就保持现状，所得现金就是昨天持有股票的所得现金 即：dp[i - 1][0]
- 第i天买入股票，所得现金就是买入今天的股票后所得现金即：-prices[i]
  那么dp[i][0]应该选所得现金最大的，所以dp[i][0] = max(dp[i - 1][0], -prices[i]);

如果第i天不持有股票即dp[i][1]， 也可以由两个状态推出来

- 第i-1天就不持有股票，那么就保持现状，所得现金就是昨天不持有股票的所得现金 即：dp[i - 1][1]
- 第i天卖出股票，所得现金就是按照今天股票佳价格卖出后所得现金即：prices[i] + dp[i - 1][0]
  同样dp[i][1]取最大的，dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);

```cpp
// 版本一
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        if (len == 0) return 0;
        vector<vector<int>> dp(len, vector<int>(2));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], prices[i] + dp[i - 1][0]);
        }
        return dp[len - 1][1];
    }
};
```

```cpp
// 版本二
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        vector<vector<int>> dp(2, vector<int>(2)); // 注意这里只开辟了一个2 * 2大小的二维数组
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i % 2][0] = max(dp[(i - 1) % 2][0], -prices[i]);
            dp[i % 2][1] = max(dp[(i - 1) % 2][1], prices[i] + dp[(i - 1) % 2][0]);
        }
        return dp[(len - 1) % 2][1];
    }
};
```

## 买卖股票的最佳时机II

[买卖股票的最佳时机II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

示例 1:
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4。随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 。

本题，因为一只股票可以买卖多次，所以当第i天买入股票的时候，所持有的现金可能有之前买卖过的利润。

那么第i天持有股票即dp[i][0]，如果是第i天买入股票，所得现金就是昨天不持有股票的所得现金 减去 今天的股票价格 即：`dp[i - 1][1] - prices[i]`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int len = prices.size();
        vector<vector<int>> dp(len, vector<int>(2, 0));
        dp[0][0] -= prices[0];
        dp[0][1] = 0;
        for (int i = 1; i < len; i++) {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]); // 注意这里是和121. 买卖股票的最佳时机唯一不同的地方。
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[len - 1][1];
    }
};
```

## 买卖股票的最佳时机III

[买卖股票的最佳时机III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii)

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

**思路**

1. 确定dp数组以及下标的含义
    一天一共就有五个状态，
    0 没有操作
    1 第一次买入
    2 第一次卖出
    3 第二次买入
    4 第二次卖出
   dp[i][j]中 i表示第i天，j为 [0 - 4] 五个状态，dp[i][j]表示第i天状态j所剩最大现金。

2. 确定递推公式
  
    需要注意：dp[i][1]，表示的是第i天，买入股票的状态，并不是说一定要第i天买入股票，这是很容易陷入的误区。
   
    **达到dp[i][1]状态，有两个具体操作：**
   
   - 操作一：第i天买入股票了，那么`dp[i][1] = dp[i-1][0] - prices[i]`
   
   - 操作二：第i天没有操作，而是沿用前一天买入的状态，即：`dp[i][1] = dp[i - 1][1]`
     那么dp[i][1]究竟选 `dp[i-1][0] - prices[i]`，还是dp[i - 1][1]呢？
     
     一定是选最大的，所以 `dp[i][1] = max(dp[i-1][0] - prices[i], dp[i - 1][1]);`
     
     **同理dp[i][2]也有两个操作：**
   
   - 操作一：第i天卖出股票了，那么`dp[i][2] = dp[i - 1][1] + prices[i]`
   
   - 操作二：第i天没有操作，沿用前一天卖出股票的状态，即：`dp[i][2] = dp[i - 1][2]`
     所以`dp[i][2] = max(dp[i - 1][1] + prices[i], dp[i - 1][2])`
     
     同理可推出剩下状态部分：
     
     `dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);`
     
     `dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);`

3. dp数组如何初始化
- 第0天没有操作，这个最容易想到，就是0，即：`dp[0][0] = 0;`

- 第0天做第一次买入的操作，`dp[0][1] = -prices[0];`

- 第0天做第一次卖出: `dp[0][2] = 0;`

- 第0天第二次买入操作，第二次买入依赖于第一次卖出的状态，所以第二次买入操作，初始化为：`dp[0][3] = -prices[0];`

- 同理第二次卖出初始化dp[0][4] = 0;

```cpp
// 版本一
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) return 0;
        vector<vector<int>> dp(prices.size(), vector<int>(5, 0));
        dp[0][1] = -prices[0];
        dp[0][3] = -prices[0];
        for (int i = 1; i < prices.size(); i++) {
            dp[i][0] = dp[i - 1][0];
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
            dp[i][2] = max(dp[i - 1][2], dp[i - 1][1] + prices[i]);
            dp[i][3] = max(dp[i - 1][3], dp[i - 1][2] - prices[i]);
            dp[i][4] = max(dp[i - 1][4], dp[i - 1][3] + prices[i]);
        }
        return dp[prices.size() - 1][4];
    }
};
```

```cpp
// 版本二
class Solution {
public:
   int maxProfit(vector<int>& prices) {
       if (prices.size() == 0) return 0;
       vector<int> dp(5, 0);
       dp[1] = -prices[0];
       dp[3] = -prices[0];
       for (int i = 1; i < prices.size(); i++) {
           dp[1] = max(dp[1], dp[0] - prices[i]);
           dp[2] = max(dp[2], dp[1] + prices[i]);
           dp[3] = max(dp[3], dp[2] - prices[i]);
           dp[4] = max(dp[4], dp[3] + prices[i]);
       }
       return dp[4];
   }
};
```

## 买卖股票的最佳时机IV

给定一个整数数组 prices ，它的第 i 个元素 prices[i] 是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int len =prices.size();
        if(len==0) return 0;
        /*
            dp[i][0]截止到第i天未进行过任何操作；

            dp[i][1]截止到第i天只进行过一次买操作；

            dp[i][2]截止到第i天进行了一次买操作和一次卖操作，即完成了一笔交易；

            dp[i][3]截止到第i天在完成了一笔交易的前提下，进行了第二次买操作；

            dp[i][4]截止到第i天完成了全部两笔交易。

            ........

        */   
        vector<vector<int>> dp(len,vector<int>(2*k+1));
        /*
        dp[0][0]=0;
        dp[0][1]=-prices[0];
        dp[0][2]=0;
        dp[0][3]=-prices[0];
        dp[0][4]=0;
        */
        // 初始化
        for(int i=0;i<2*k+1;++i){
            if(i%2==1) dp[0][i]=-prices[0];
            else dp[0][i]=0;
        }
        for(int i=1;i<len;++i){
            dp[i][0]=dp[i-1][0];
            for(int j=1;j<=k;++j ){
                dp[i][j*2-1]=max(dp[i-1][j*2-1],dp[i-1][j*2-2]-prices[i]);
                dp[i][j*2]=max(dp[i-1][j*2-1]+prices[i],dp[i-1][j*2]);

            }
        }
        return dp[len-1][2*k];
    }
};
```

## 最长递增子序列

[最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence)

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

    输入：nums = [10,9,2,5,3,7,101,18]
    输出：4
    解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 

**思路：**

最长上升子序列是动规的经典题目，这里dp[i]是可以根据dp[j] （j < i）推导出来的，那么依然用动规五部曲来分析详细一波

1. dp[i]的定义
   dp[i]表示i之前包括i的以nums[i]结尾最长上升子序列的长度

2. 状态转移方程
   位置i的最长升序子序列等于j从0到i-1各个位置的最长升序子序列 + 1 的最大值。
   所以：if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);

**注意这里不是要dp[i] 与 dp[j] + 1进行比较，而是我们要取dp[j] + 1的最大值。**

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() <= 1) return nums.size();
        vector<int> dp(nums.size(), 1);
        int result = 0;
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);
            }
            if (dp[i] > result) result = dp[i]; // 取长的子序列
        }
        return result;
    }
};
```

## 718. 最长重复子数组

 给两个整数数组 nums1 和 nums2 ，返回 两个数组中 公共的 、长度最长的子数组的长度 。

    输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
    输出：3
    解释：长度最长的公共子数组是 [3,2,1] 

```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {     
        vector<vector<int>> dp(nums1.size()+1,vector<int>(nums2.size()+1,0));
        int res=0;
        for(int i=1;i<=nums1.size();++i){
            for(int j=1;j<=nums2.size();++j){
                if(nums1[i-1]==nums2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }
                if(res<dp[i][j]) res=dp[i][j];

            }
        }
        return res;
    }
};
```

## [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)

该算法采用了动态规划的思想求解两个字符串的最长公共子序列。具体解释如下：

```markdown
该算法采用了动态规划的思想求解两个字符串的最长公共子序列。具体解释如下：

- 创建一个二维动态规划数组dp，其中dp[i][j]表示text1[0:i-1]和text2[0:j-1]的最长公共子序列长度。初始化dp数组为0。
- 创建一个二维数组lcs，其中lcs[i][j]表示text1[0:i-1]和text2[0:j-1]的最长公共子序列。初始化lcs数组为空字符串。
- 遍历text1和text2的所有组合，如果当前text1[i-1]和text2[j-1]相等，则它们可以作为公共子序列的一部分。
    此时更新dp[i][j]为dp[i-1][j-1]+1，并更新lcs[i][j]为lcs[i-1][j-1]+text1[i-1]；
- 如果text1[i-1]和text2[j-1]不相等，则它们不能同时作为公共子序列的一部分。
    此时更新dp[i][j]为dp[i-1][j]和dp[i][j-1]中的较大值，并将lcs[i][j]的值由dp[i-1][j]或dp[i][j-1]中的值转移而来。
- 遍历完成后，返回dp[text1.size()][text2.size()]即为text1和text2的最长公共子序列长度，
    输出lcs[text1.size()][text2.size()]即为text1和text2的最长公共子序列。
```

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        // 定义dp数组和lcs数组，大小分别是(text1.size()+1)×(text2.size()+1)，并初始化为0和空字符串
        vector<vector<int>> dp(text1.size()+1,vector<int>(text2.size()+1,0));
        vector<vector<string>> lcs(text1.size()+1,vector<string>(text2.size()+1,""));

        // 动态规划求解最长公共子序列
        for(int i=1;i<=text1.size();++i){
            for(int j=1;j<=text2.size();++j){
                // 如果text1[i-1]和text2[j-1]相等，则它们可以作为公共子序列的一部分
                if(text1[i-1]==text2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;  // 更新dp[i][j]的值为dp[i-1][j-1]+1
                    lcs[i][j]=lcs[i-1][j-1]+text1[i-1];  // 更新lcs[i][j]的值为lcs[i-1][j-1]+text1[i-1]
                }
                else{
                    // 如果text1[i-1]和text2[j-1]不相等，则它们不能同时作为公共子序列的一部分
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);  // dp[i][j]的值由dp[i-1][j]和dp[i][j-1]中的较大值决定
                    if(dp[i][j]==dp[i-1][j]) lcs[i][j]=lcs[i-1][j];  // 如果dp[i][j]由dp[i-1][j]转移而来，则lcs[i][j]也由lcs[i-1][j]转移而来
                    else lcs[i][j]=lcs[i][j-1];  // 否则，lcs[i][j]由lcs[i][j-1]转移而来
                }
            }
        }
        cout<<lcs[text1.size()][text2.size()]<<endl;  // 输出最长公共子序列
        return dp[text1.size()][text2.size()];  // 返回text1和text2的最长公共子序列长度
    }
};
```
