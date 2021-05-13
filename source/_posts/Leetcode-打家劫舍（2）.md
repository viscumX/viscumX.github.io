---
title: Leetcode-打家劫舍（2）
date: 2021-04-15 10:56:27
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都围成一圈，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你在不触动警报装置的情况下 ，能够偷窃到的最高金额。

**刷题链接**：https://leetcode-cn.com/problems/house-robber-ii

<!--more-->

## 解法

动态规划（dp），数组首尾相接，可以把它看成一个去掉头的数组和一个去掉尾的数组。选择当前值就不能选择前一个值，故状态转移为`dp[i+1] = max(dp[i-1]+nums[i], dp[i])`

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n==1){
            return nums[0];
        }
        vector<int> dp(n);
        dp[0] = 0;
        for(int i = 0;i<n-1;++i){
            if(i==0){
                dp[i+1] = nums[i];
            }
            else{
                dp[i+1] = max(dp[i-1]+nums[i], dp[i]);
            }
        }
        int tmp = dp[n-1];
        for(int i = 1;i<n;++i){
            if(i==1){
                dp[i] = nums[i];
            }
            else{
                dp[i] = max(dp[i-2]+nums[i], dp[i-1]);
            }
        }
        return max(dp[n-1], tmp);
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$，也可以通过两个指针，将空间复杂度降到$O(1)$
