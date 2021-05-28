---
title: 剑指offer-矩阵中的路径
date: 2021-05-28 18:07:25
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个  m x n 二维字符网格  board 和一个字符串单词  word 。如果  word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**刷题链接**：[https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof)

<!--more-->

## 解法

可以采用深度搜索+剪枝的思想，递推时先标记当前元素为空字符，防止重复访问，验证过其他路径后再还原

```C++
class Solution {
public:
    bool exist(vector<vector<char> >& matrix, string word) {
        // write code here
        rows = matrix.size();
        cols = matrix[0].size();
        for(int i=0;i<rows;++i){
            for(int j=0;j<cols;++j){
                if(dfs(matrix, word, i, j, 0)){
                    return true;
                }
            }
        }
        return false;
    }
private:
    int rows, cols;
    bool dfs(vector<vector<char>> &matrix, string word, int i, int j, int k){
        if(i<0||i>=rows||j<0||j>=cols||matrix[i][j]!=word[k]){
            return false;
        }
        if(k==word.size()-1){
            return true;
        }
        matrix[i][j] = '\0';
        bool res = dfs(matrix, word, i-1,j,k+1) || dfs(matrix, word, i+1,j,k+1)
            || dfs(matrix, word, i,j-1,k+1) || dfs(matrix, word, i,j+1,k+1);
        matrix[i][j] = word[k];
        return res;
    }
};
```
