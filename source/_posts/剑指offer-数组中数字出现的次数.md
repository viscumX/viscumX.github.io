---
title: 剑指offer-数组中数字出现的次数
date: 2021-05-20 19:14:37
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是$O(n)$，空间复杂度是$O(1)$。

刷题链接：[https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof/)

<!--more-->

## 解法

先对所有的数组内的数字进行异或，得到两个出现 1 次的数字的异或。在异或结果中找到任意一位为 1 的位，根据这一位对所有的数字进行分组，在每个组中进行异或操作

```C++
class Solution {
public:
    vector<int> singleNumbers(vector<int>& nums) {
        int res = 0;
        for(int n : nums){
            res ^= n;
        }
        int div = 1;
        while((div & res)==0){
            div <<= 1;
        }
        int a = 0;
        int b = 0;
        for(int n : nums){
            if(div & n){
                a ^= n;
            }
            else{
                b ^= n;
            }
        }
        return vector<int>{a, b};
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
