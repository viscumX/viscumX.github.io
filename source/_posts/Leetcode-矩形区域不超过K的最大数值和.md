---
title: Leetcode-矩形区域不超过K的最大数值和
date: 2021-05-17 15:19:05
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

一个 m x n 的矩阵 matrix 和一个整数 k ，找出并返回矩阵内部矩形区域的不超过 k 的最大数值和。保证总会存在一个数值和不超过 k 的矩形区域。

**刷题链接**：[https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k](https://leetcode-cn.com/problems/max-sum-of-rectangle-no-larger-than-k)

<!--more-->

## 解法

枚举矩阵的上下边界，计算范围内的每列和，就能把问题转换为一维数组的问题。在枚举有边界的同时，要使左边界尽量靠左，这样就能得到一个比较大的矩阵

```C++
class Solution {
public:
    int maxSumSubmatrix(vector<vector<int>> &matrix, int k) {
        int ans = INT_MIN;
        int m = matrix.size()
        int n = matrix[0].size();
        for (int i = 0; i < m; ++i) {
            vector<int> sum(n);
            for (int j = i; j < m; ++j) {
                for (int c = 0; c < n; ++c) {
                    sum[c] += matrix[j][c];
                }
                set<int> sumSet{0};
                int s = 0;
                for (int v : sum) {
                    s += v;
                    auto lb = sumSet.lower_bound(s - k);
                    if (lb != sumSet.end()) {
                        ans = max(ans, s - *lb);
                    }
                    sumSet.insert(s);
                }
            }
        }
        return ans;
    }
};
```

假设矩阵的行为 m，列为 n，则时间复杂度为$O(m^2nlogn)$，空间复杂度为$O(n)$
