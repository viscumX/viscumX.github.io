---
title: 剑指offer-构建乘积数组
date: 2021-05-24 22:42:14
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中  B[i] 的值是数组 A 中除了下标 i 以外的元素的积, 即  B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

**刷题链接**：[https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof](https://leetcode-cn.com/problems/gou-jian-cheng-ji-shu-zu-lcof)

<!--more-->

## 解法

构建矩阵，分为上三角和下三角分别计算乘积

```C++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {
        vector<int> res(a.size(), 1);
        for(int i=1;i<a.size();++i){
            res[i] = res[i-1]*a[i-1];
        }
        int tmp = 1;
        for(int j = a.size()-2;j>=0;--j){
            tmp *= a[j+1];
            res[j] *= tmp;
        }
        return res;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
