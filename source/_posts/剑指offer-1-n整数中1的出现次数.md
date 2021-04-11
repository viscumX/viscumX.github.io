---
title: 剑指offer-1~n整数中1的出现次数
date: 2021-04-11 20:37:45
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

输入一个整数 n ，求 1 ~ n 这 n 个整数的十进制表示中 1 出现的次数。

例如，输入 12，1 ~ 12 这些整数中包含 1 的数字有 1、10、11 和 12，1 一共出现了 5 次。

**刷题链接**：[https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof)

<!--more-->

## 解法

分当前位为 0，1 和其他三种情况讨论，挺复杂的题目

```C++
class Solution {
public:
    int NumberOf1Between1AndN_Solution(int n){
        int res = 0, digit = 1, high = n/10, cur = n%10, low = 0;
        while(high || cur){
            if(cur==0){
                res += high*digit;
            }
            else if(cur==1){
                res += high*digit+low+1;
            }
            else{
                res += (high+1)*digit;
            }
            low += cur * digit;
            cur = high % 10;
            high /= 10;
            digit *= 10;
        }
        return res;
    }
};
```

时间复杂度为$O(logN)$，空间复杂度为$O(1)$
