---
title: Leetcode-存在重复元素（3）
date: 2021-04-18 13:44:46
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

一个整数数组 nums 和两个整数  k 和 t ，判断是否存在 两个不同下标 i 和 j，使得  abs(nums[i] - nums[j]) <= t ，同时又满足 abs(i - j) <= k 。

**刷题链接**：[https://leetcode-cn.com/problems/contains-duplicate-iii](https://leetcode-cn.com/problems/contains-duplicate-iii)

<!--more-->

## 解法

### 滑动窗口+有序集合

如果 nums 中的某个数 x 左侧的 k 个元素中有处于$[x-t, x+t]$之间的，就满足条件。如果未满足条件，就将 x 加入到集合中，如果集合中的元素数超过了 k，就删除掉最先加入的数

```C++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        int n = nums.size();
        set<int> rec;
        for (int i = 0; i < n; i++) {
            auto iter = rec.lower_bound(max(nums[i], INT_MIN + t) - t);
            if (iter != rec.end() && *iter <= min(nums[i], INT_MAX - t) + t) {
                return true;
            }
            rec.insert(nums[i]);
            if (i >= k) {
                rec.erase(nums[i - k]);
            }
        }
        return false;
    }
};
```

时间复杂度为$O(Nlogk)$，空间复杂度为$O(k)$

### 桶

要把 k 个数字放到桶里，对于非负数 x，除以 t+1 可以让相差小于等于 t 的数都落在同一个桶里；对于负数，需要将负数整体右移，即 x+1 才能做到。

对于某个数组 x，假设之前已经存在对应编号的桶，则说明已经有$[x-t, x+t]$之间的数。或者检查相邻桶，是否存在与 x 相差不超过 t 的数。如果找不到这样的数，就建立目标桶，并且移除下标范围不在`[max(0, i-k), i)`中的桶

```C++
class Solution {
public:
    int getID(int x, long w) {
        return x < 0 ? (x + 1ll) / w - 1 : x / w;
    }

    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        unordered_map<int, int> mp;
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            long x = nums[i];
            int id = getID(x, t + 1ll);
            if (mp.count(id)) {
                return true;
            }
            if (mp.count(id - 1) && abs(x - mp[id - 1]) <= t) {
                return true;
            }
            if (mp.count(id + 1) && abs(x - mp[id + 1]) <= t) {
                return true;
            }
            mp[id] = x;
            if (i >= k) {
                mp.erase(getID(nums[i - k], t + 1ll));
            }
        }
        return false;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(k)$
