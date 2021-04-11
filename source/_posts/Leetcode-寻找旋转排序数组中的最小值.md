---
title: Leetcode-寻找旋转排序数组中的最小值
date: 2021-04-10 17:26:58
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次旋转后，得到输入数组

数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]]

nums 是一个元素值互不相同的数组，原来是升序排列的，并按上述情形进行了多次旋转。请找出并返回数组中的最小元素

**刷题链接**：[https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array)

<!--more-->

## 解法

### 暴力查找

遍历硬找

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int res = nums[0];
        int i = 1;
        while(i<nums.size()){
            if(res>nums[i]){
                res = nums[i];
                break;
            }
            ++i;
        }
        return res;
    }
};
```

时间复杂度最坏为$O(N)$，空间复杂度为$O(1)$

### 二分查找

数组最小值左边的元素一定都大于数组的最后一个元素，右边的元素一定都小于数组的最后一个元素。假设 mid 小于 r 说明最小值在当前区间的左半，否则就在右半边

```C++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size()-1;
        while(l<r){
            int mid = (l+r)/2;
            if(nums[mid]<nums[r]){
                r = mid;
            }
            else{
                l = mid+1;
            }
        }
        return nums[l];
    }
};
```

时间复杂度为$O(logN)$，空间复杂度为$O(1)$
