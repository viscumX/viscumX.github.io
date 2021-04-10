---
title: Leetcode刷题-搜索旋转排序数组（2）
date: 2021-04-07 13:51:45
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

已知存在一个按非降序排列的整数数组 nums，数组中的值不必互不相同。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了旋转，使数组变为[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标从 0 开始计数）。例如，[0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为[4,5,6,6,7,0,1,2,4,4] 。

给你旋转后的数组 nums 和一个整数 target，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target，则返回 true，否则返回 false

**刷题链接**：[https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii)

<!--more-->

## 解法

### 二分查找

暴力搜索是可行的，但是为了节省时间，可以采用二分查找的方法。假设 mid 大于等于 l，说明左边是单调不减，否则说明右边单调不减。因为有重复元素，所以可能 l，mid，r 都相等，因而无法判断哪个区间是有序的，所以要左边界加一，右边界减一

```C++
class Solution {
public:
    bool search(vector<int> &nums, int target) {
        if (nums.size() == 0) {
            return false;
        }
        if (nums.size() == 1) {
            return nums[0] == target;
        }
        int l = 0, r = nums.size() - 1;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[l] == nums[mid] && nums[mid] == nums[r]) {
                ++l;
                --r;
            } else if (nums[l] <= nums[mid]) {
                if (nums[l] <= target && target < nums[mid]) {
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            } else {
                if (nums[mid] < target && target <= nums[n - 1]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
            }
        }
        return false;
    }
};
```

时间复杂度最坏为$O(N)$，空间复杂度为$O(1)$
