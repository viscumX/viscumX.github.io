---
title: Leetcode-子集（2）
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
        for(int i = 0; i < nums.size(); i++){
            int times = numCount(nums, nums[i], i);
            vector<vector<int>> tmp2(res);
            for(auto &v: tmp2){
                if(!times || (numCount(v, nums[i], v.size()) == times)){
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

### 迭代法

非常高级的实现，先将数组排序，若发现没有选择上一个数，且当前数字与上一个数相同，则可以跳过当前生成的子集。排序后使用掩膜，表示选择某个数和不选择某个数的两种状态

```C++
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        vector<vector<int>> res;
        vector<int> tmp;
        sort(nums.begin(), nums.end());
        for (int mask = 0; mask < (1 << nums.size()); ++mask) {
            tmp.clear();
            bool flag = true;
            for (int i = 0; i < nums.size(); ++i) {
                if (mask & (1 << i)) {
                    if (i > 0 && (mask >> (i - 1) & 1) == 0 && nums[i] == nums[i - 1]) {
                        flag = false;
                        break;
                    }
                    tmp.push_back(nums[i]);
                }
            }
            if (flag) {
                res.push_back(t);
            }
        }
        return res;
    }
};
```

时间复杂度为$O(N*2^N)$，空间复杂度为$O(N)$

### 递归法

思路基本一致，但是采用了递归的方法

```C++
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> subsetsWithDup(vector<int> &nums) {
        sort(nums.begin(), nums.end());
        dfs(false, 0, nums);
        return res;
    }

private:
    void dfs(bool pre, int cur, vector<int> &nums) {
        vector<int> tmp;
        if (cur == nums.size()) {
            res.push_back(tmp);
            return;
        }
        dfs(false, cur + 1, nums);
        if (cur > 0 && !pre && nums[cur - 1] == nums[cur]) {
            return;
        }
        tmp.push_back(nums[cur]);
        dfs(true, cur + 1, nums);
        tmp.pop_back();
    }
};
```

时间复杂度为$O(N*2^N)$，空间复杂度为$O(N)$
