---
title: Leetcode刷题-子集（2）
date: 2021-03-31 16:40:19
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集不能包含重复的子集。返回的解集中，子集可以按任意顺序排列。

**刷题链接**：[https://leetcode-cn.com/problems/subsets-ii](https://leetcode-cn.com/problems/subsets-ii)

<!--more-->

## 解法

### 枚举去重

如果当前数字出现过，计算当前数字在它之前出现的次数，只要在出现过当前数字的子集中加上当前数字就能形成新的子集。对于未出现过的数字，只要在当前所有的子集中加入当前数字就可以形成新的子集

```C++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> tmp1;
        res.emplace_back(tmp1);
        for(int i = 0; i<nums.size(); i++){
            int times = numCount(nums, nums[i], i);
            vector<vector<int>> tmp2(res);
            for(auto &v: tmp2){
                if(!times||(numCount(v, nums[i], v.size())==times)){
                    tmp1 = v;
                    tmp1.emplace_back(nums[i]);
                    res.emplace_back(tmp1);
                }
            }
        }
        return res;
    }

private:
    int numCount(vector<int> &nums, int num, int end){
        if(!end){
            return 0;
        }
        if(nums[end-1]==num){
            return numCount(nums, num, end-1)+1;
        }
        else{
            return numCount(nums, num, end-1);
        }
    }
};
```
