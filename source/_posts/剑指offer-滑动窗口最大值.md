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

假设`nums[i] <= nums[j]`，且`i < j`，那么只要 nums[i]还在窗口内，nums[j]一定也在窗口内，可以将 nums[i]移除。当窗口向右移动时，将新元素比队尾元素大，就可以将队尾元素移除，直到队列为空或队尾元素大于新元素。弹出队首不在窗口内的元素，队首元素就应该是滑动窗口的最大值

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        deque<int> q;
        for(int i=0;i<k;++i){
            while(!q.empty()&&nums[i]>=nums[q.back()]){
                q.pop_back();
            }
            q.push_back(i);
        }

        vector<int> res = {nums[q.front()]};
        for(int i=k;i<n;++i){
            while(!q.empty()&&nums[i]>=nums[q.back()]){
                q.pop_back();
            }
            q.push_back(i);
            while(q.front()<=i-k){
                q.pop_front();
            }
            res.push_back(nums[q.front()]);
        }
        return res;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(k)$

### 分块+预处理

分为 k 个元素一组

- 如果 i 是 k 的倍数，只要预处理出每个分组的最大值，即可得到答案
- 如果 i 不是 k 的倍数，那就会横跨两个分组

```C++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> prefixMax(n), suffixMax(n);
        for (int i = 0; i < n; ++i) {
            if (i % k == 0) {
                prefixMax[i] = nums[i];
            }
            else {
                prefixMax[i] = max(prefixMax[i - 1], nums[i]);
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            if (i == n - 1 || (i + 1) % k == 0) {
                suffixMax[i] = nums[i];
            }
            else {
                suffixMax[i] = max(suffixMax[i + 1], nums[i]);
            }
        }

        vector<int> res;
        for (int i = 0; i <= n - k; ++i) {
            res.push_back(max(suffixMax[i], prefixMax[i + k - 1]));
        }
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
