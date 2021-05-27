---
title: 剑指offer-滑动窗口最大值
date: 2021-05-27 20:12:14
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

整数数组 nums，有一个大小为  k  的滑动窗口从数组的最左侧移动到数组的最右侧。可以看到在滑动窗口内的 k  个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

**刷题链接**：[https://leetcode-cn.com/problems/sliding-window-maximum](https://leetcode-cn.com/problems/sliding-window-maximum)

<!--more-->

## 解法

### 优先队列

暴力就直接不看了

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        priority_queue<pair<int, int>> maxHeap;
        for(int i = 0; i < k; ++i){
            maxHeap.emplace(nums[i], i);
        }
        vector<int> res = {maxHeap.top().first};
        for(int i=k;i<n;++i){
            maxHeap.emplace(nums[i], i);
            while(maxHeap.top().second <= i-k){
                maxHeap.pop();
            }
            res.emplace_back(maxHeap.top().first);
        }
        return res;
    }
};
```

时间复杂度为$O(NlogN)$，空间复杂度$O(N)$

### 单调队列
