---
title: Leetcode-寻找旋转排序数组中的最小值（2）
date: 2021-04-11 12:15:29
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次旋转后，得到输入数组

数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]]

数组 nums 可能存在重复的元素，原来是升序排列的，并按上述情形进行了多次旋转。请找出并返回数组中的最小元素

**刷题链接**：[https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)

<!--more-->

## 解法

同样是二分查找。去重时，假设 mid 和 r 相等，对于可能存在重复元素的数组，可以忽略区间的右端点，因为有 mid 的存在

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0;
        int r = nums.size()-1;
        while(l<r){
            int mid = (l+r)>>1;
            if(nums[mid]<nums[r]){
                r = mid;
            }
            else if(nums[mid]>nums[r]){
                l = mid+1;
            }
            else{
                r -= 1;
            }
        }
        return nums[l];
    }
};
```

最坏时间复杂度为$O(N)$，空间复杂度为$O(1)$
