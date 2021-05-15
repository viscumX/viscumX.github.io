---
title: Leetcode-最大整除子集
date: 2021-05-15 16:08:40
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

一个由**无重复**正整数组成的集合 nums，找出并返回其中最大的整除子集 answer，子集中每一元素对 (answer[i], answer[j]) 都应当满足：

- answer[i] % answer[j] == 0 ，或 answer[j] % answer[i] == 0

如果存在多个有效解子集，返回其中任何一个均可。

**刷题链接**：[https://leetcode-cn.com/problems/largest-divisible-subset](https://leetcode-cn.com/problems/largest-divisible-subset)

<!--more-->

## 解法

又是经典 dp 大法。

如果 x 是某一整除子集中的最小整数的约数或某一整除子集中的最大整数的倍数，就可以加入到整除子集中。

先对数组做升序排列，dp[i] 表示在以 nums[i] 为最大整数的整除子集的大小。如果 nums[j]（0<=j<=i-1）能整除 nums[i] ，就说明 nums[j] 可以扩充以 nums[j] 为最大整数的整除子集。

为了得到目标子集，首先要得到最大子集长度，然后倒着遍历 dp 数组，找到对应着最大子集长度的 nums[i]，然后将最大子集长度减小，继续向前寻找对应的 nums[i]

```C++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        vector<int> dp(len, 1);
        int maxSize = 1;
        int maxVal = dp[0];
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }

            if (dp[i] > maxSize) {
                maxSize = dp[i];
                maxVal = nums[i];
            }
        }

        vector<int> res;
        if (maxSize == 1) {
            res.push_back(nums[0]);
            return res;
        }

        for (int i = nums.size() - 1; i >= 0 && maxSize > 0; i--) {
            if (dp[i] == maxSize && maxVal % nums[i] == 0) {
                res.push_back(nums[i]);
                maxVal = nums[i];
                maxSize--;
            }
        }
        return res;
    }
};
```

时间复杂度为$O(N^2)$，空间复杂度为$O(N)$
