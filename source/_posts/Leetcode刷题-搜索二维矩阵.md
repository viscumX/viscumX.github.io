---
title: Leetcode刷题-搜索二维矩阵
date: 2021-03-30 13:17:05
categories: 刷题记录
tags: 刷题
---

## 题目描述

编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

- 每行中的整数从左到右按升序排列。
- 每行的第一个整数大于前一行的最后一个整数。

**刷题链接**：[https://leetcode-cn.com/problems/search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix)

<!--more-->

## 解法

### 两次二分查找

第一次二分查找，target 可能在的行，第二次在该行内进行二分查找

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int rowBegin = 0;
        int rowEnd = matrix.size()-1;
        int colBegin = 0;
        int colEnd = matrix[0].size()-1;
        while(rowBegin<=rowEnd){
            int rowMid = (rowBegin+rowEnd)/2;
            if(matrix[rowMid][colBegin]>target){
                rowEnd = rowMid-1;
            }
            else if(matrix[rowMid][colEnd]<target){
                rowBegin = rowMid+1;
            }
            else{
                while(colBegin<=colEnd){
                    int colMid = (colBegin+colEnd)/2;
                    if(matrix[rowMid][colMid]<target){
                        colBegin = colMid+1;
                    }
                    else if(matrix[rowMid][colMid]==target){
                        return true;
                    }
                    else{
                        colEnd = colMid-1;
                    }
                }
                break;
            }
        }
        return false;
    }
};
```

假设矩阵长为 M，宽为 N，则时间复杂度为$O(logMN)$，空间复杂度为$O(1)$

### 一次二分查找

因为矩阵的升序特性，可以将它拉成一个数组，然后用一次二分查找就可以了

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int mBegin = 0;
        if(!matrix.size()){
            return false;
        }
        int cols = matrix[0].size();
        int mEnd = matrix.size()*(matrix[0].size())-1;
        while(mBegin<=mEnd){
            int mMid = (mBegin+mEnd)/2;
            if(matrix[mMid/cols][mMid%cols] < target){
                mBegin = mMid+1;
            }
            else if(matrix[mMid/cols][mMid%cols] > target){
                mEnd = mMid-1;
            }
            else{
                return true;
            }
        }
        return false;
    }
};
```

假设矩阵长为 M，宽为 N，则时间复杂度为$O(logMN)$，空间复杂度为$O(1)$

### 贪心算法

从矩阵的左下角（或右上角）开始，比 target 大就往上，比 target 小就往右

```C++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int i = matrix.size()-1;
        int j = 0;
        while(i>=0&&j<matrix[0].size()){
            if(target==matrix[i][j]){
                return true;
            }
            else if(target>matrix[i][j]){
                j += 1;
            }
            else{
                i -= 1;
            }
        }
        return false;
    }
};
```

假设矩阵长为 M，宽为 N，则时间复杂度最坏为$O(M+N)$，空间复杂度为$O(1)$
