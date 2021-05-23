---
title: 剑指offer-不用加减乘除做加法
date: 2021-05-23 23:11:29
categories: 刷题记录
tags: 刷题
---

## 题目描述

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“\*”、“/” 四则运算符号。

**刷题链接**：[https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/](https://leetcode-cn.com/problems/bu-yong-jia-jian-cheng-chu-zuo-jia-fa-lcof/)

<!--more-->

## 解法

位运算，两个数异或等于相加而不考虑进位，两个数相与相当于进位

```C++
class Solution {
public:
    int add(int a, int b) {
        while(b){
            int sum = a^b;
            int carry = (a & b) << 1;
            a = sum;
            b = carry;
        }
        return a;
    }
};
```
