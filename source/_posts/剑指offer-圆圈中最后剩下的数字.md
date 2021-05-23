---
title: 剑指offer-圆圈中最后剩下的数字
date: 2021-05-23 22:19:05
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

0,1,···,n-1 这 n 个数字排成一个圆圈，从数字 0 开始，每次从这个圆圈里删除第 m 个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4 这 5 个数字组成一个圆圈，从数字 0 开始每次删除第 3 个数字，则删除的前 4 个数字依次是 2、0、4、1，因此最后剩下的数字是 3

**刷题链接**：[https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof)

<!--more-->

## 解法

### 递归

当剩下最后一个数的时候，它的编号一定为 0

```C++
class Solution {
    int f(int n, int m) {
        if (n == 1) {
            return 0;
        }
        return (m + f(n - 1, m)) % n;
    }
public:
    int lastRemaining(int n, int m) {
        return f(n, m);
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 迭代

将之前的递归改写为迭代

```C++
class Solution {
public:
    int lastRemaining(int n, int m) {
        int f = 0;
        for(int i = 2; i!= n + 1; ++i){
            f = (m + f) % i;
        }
        return f;
    }
}
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
