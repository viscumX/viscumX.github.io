---
title: Leetcode刷题-直方图的水量
date: 2021-04-02 19:13:19
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个直方图(也称柱状图)，假设有人从上面源源不断地倒水，最后直方图能存多少水量?直方图的宽度为 1。

**刷题链接**：[https://leetcode-cn.com/problems/volume-of-histogram-lcci/](https://leetcode-cn.com/problems/volume-of-histogram-lcci/)

<!--more-->

## 解法

假设右边存在比当前左边高的高度，存水量就取决于左边的高度，同理右边也一样。用双指针的方法对 height 进行遍历

```C++
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        if(height.size()<4){
            return res;
        }
        int left = 0;
        int right = height.size()-1;
        int leftMax = 0;
        int rightMax = 0;
        while(left<=right){
            if(leftMax<rightMax){
                res += max(0, leftMax-height[left]);
                leftMax = max(leftMax, height[left]);
                left++;
            }
            else{
                res += max(0, rightMax-height[right]);
                rightMax = max(rightMax, height[right]);
                right--;
            }
        }
        return res;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
