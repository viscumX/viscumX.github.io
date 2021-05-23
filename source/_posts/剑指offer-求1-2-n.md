---
title: 剑指offer-求1+2+...+n
date: 2021-05-23 22:34:11
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case 等关键字及条件判断语句（A?B:C）

**刷题链接**：[https://leetcode-cn.com/problems/qiu-12n-lcof/](https://leetcode-cn.com/problems/qiu-12n-lcof/)

<!--more-->

## 解法

### 递归

利用逻辑运算符`&&`

```C++
class Solution {
public:
    int sumNums(int n) {
        bool x = n && (n += sumNums(n-1));
        return n;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 快速乘

a 和 b 两数相乘可以用加法和位运算模拟。将 b 二进制展开，假设 b 的第 i 位为 1，那么就意味着`a * (1 << i)`，即`a << i`，那么遍历 b 的每一位并把结果相加就能得到`a * b`的结果

```C++
class Solution {
public:
    int sumNums(int n) {
        int ans = 0, A = n, B = n + 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        (B & 1) && (ans += A);
        A <<= 1;
        B >>= 1;

        return ans >> 1;
    }
};
```

时间复杂度为$O(logN)$，空间复杂度为$O(1)$
