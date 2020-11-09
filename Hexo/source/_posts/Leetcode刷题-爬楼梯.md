---
title: Leetcode刷题-爬楼梯
date: 2020-11-09 15:13:55
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

假设你正在爬楼梯。需要 n 阶你才能到达楼顶

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

刷题链接：[https://leetcode-cn.com/problems/climbing-stairs/](https://leetcode-cn.com/problems/climbing-stairs/)

<!--more-->

## 解法

### 数学法

或者说斐波那契数列法？关于这个爬楼梯的问题，可以分解为两步，首先爬 1 阶楼梯，接下来就是爬（n-1）阶楼梯的问题，或者首先爬 2 阶楼梯，接下来就变成了爬（n-2）阶楼梯的问题。所以整个问题就变成了爬（n-1）阶楼梯的方法数和爬（n-2）阶楼梯的方法数之和

可以用动态规划的思想

```C++
class Solution {
public:
    int climbStairs(int n) {
        int stairs1 = 0;
        int stairs2 = 1;
        int stairs3;
        for(int i = 1; i <= n; i++){
            stairs3 = stairs1 + stairs2;
            stairs1 = stairs2;
            stairs2 = stairs3;
        }
        return stairs3;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$

也可以用递归的方法来实现

```C++
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1){
            return 1;
        }
        else if (n == 2){
            return 2;
        }
        else{
            return climbStairs(n-1)+climbStairs(n-2);
        }
    }
};
```

结果超时了，逻辑应该没错，时间复杂度差不多有$O(N^2)$

还可以用公式来解，这个就太简单了，不贴代码了

### 矩阵快速幂
