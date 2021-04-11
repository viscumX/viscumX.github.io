---
title: 剑指offer-二进制中的1
date: 2021-01-13 16:28:23
categories: 刷题记录
tags: 刷题
---

## 题目描述

输入一个整数，输出该数 32 位二进制表示中 1 的个数。其中负数用补码表示

**刷题链接**：[https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

<!--more-->

## 解法

### 判负左移

负数的第一位符号位为 1，所以只要为负数就计数并左移

```C++
class Solution {
public:
     int  NumberOf1(int n) {
         int count = 0;
         while(n){
             if(n<0){
                 count++;
             }
             n<<=1;
         }
         return count;
     }
};
```

### 与 1 右移

只要最后一位为 1，和 1 相与的结果就是 1

```C++
class Solution {
public:
     int  NumberOf1(int n) {
         int count = 0;
         while(n){
             count+=n&1;
             n>>=1;
         }
         return count;
     }
};
```

### 和（n-1）相与

n 与（n-1）相与就会把最后一位 1 置零

```C++
class Solution {
public:
     int  NumberOf1(int n) {
         int count = 0;
         while(n){
             count++;
             n = n&(n-1);
         }
         return count;
     }
};
```
