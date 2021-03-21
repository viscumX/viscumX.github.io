---
title: Leetcode刷题-矩阵置零
date: 2021-03-21 12:29:19
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用原地算法，仅使用常量空间解决

**刷题链接**：[https://leetcode-cn.com/problems/set-matrix-zeroes](https://leetcode-cn.com/problems/set-matrix-zeroes)

<!--more-->

## 解法

由每一列的第一个数记录这列中是否有 0，每行的第一个数记录这列中是否有 0，那么就需要至少一个额外的标志位来记录第一行/列是否有 0

```C++
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int flag = 0;
        for(int i=0;i<matrix.size();i++){
            if(!matrix[i][0]){
                flag = 1;
            }
            for(int j=1;j<matrix[0].size();j++){
                if(!matrix[i][j]){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for(int i=matrix.size()-1;i>=0;i--){
            for(int j=1;j<matrix[0].size();j++){
                if(!matrix[i][0]||!matrix[0][j]){
                    matrix[i][j] = 0;
                }
            }
            if(flag){
                matrix[i][0] = 0;
            }
        }
    }
};
```

时间复杂度为$O(N^2)$，至多要遍历两次矩阵；空间复杂度为$O(1)$