---
title: Leetcode-最大子序和
date: 2020-11-06 16:04:49
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个整数数组 nums，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和

**刷题链接**：[https://leetcode-cn.com/problems/maximum-subarray/](https://leetcode-cn.com/problems/maximum-subarray/)

<!--more-->

## 解法

### 超时法

最直接最暴力的方法，算出所有的子序列的和，然后选出一个最大，个人感觉会超时，所以没尝试过，只在这里贴一下代码

```C++
class Solution
{
public:
    int maxSubArray(vector<int> &nums)
    {
        int max = nums[0];
        int numsize = nums.size();
        for (int i=0;i<numsize;i++){
            int sum = 0;
            for (int j=i;j<numsize;j++){
                sum += nums[j];
                if (sum>max){
                    max = sum;
                }
            }
        }
        return max;
    }
};
```

时间复杂度直接到了$O(N^2)$ ，空间复杂度为$O(1)$

### 贪心法

或者也算一种动态规划，核心思想就是记录下以当前位置结尾的最大子序列和，与总体的最大子序列和比较

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max = nums[0];
        int temp = 0;
        for (int i = 0;i<nums.size();i++){
            temp += nums[i];
            if(temp>max){
                max = temp;
            }
            if(temp<0){
                temp = 0;
            }
        }
        return max;
    }
};
```

时间复杂度为 $O(N)$ ，空间复杂度为 $O(1)$

### 分治法

虽然不是最优的方法，但是也挺难的

将 nums 分为[start, mid]和[mid+1, end]两部分，算出前后两个最大子列和值，再加上任意包含 mid 和 mid+1 的子列的最大和值，三个值取最大的

```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        return maxRangedArray(nums, 0, nums.size()-1);
    }

    int maxRangedArray(vector<int>& nums, int start, int end){
        if (start == end){
            return nums[start];
        }

        int mid = (start + end) / 2;
        int lMax = maxRangedArray(nums, start, mid);
        int rMax = maxRangedArray(nums, mid + 1, end);

        int leftMidMax = nums[mid];
        int leftSum = 0;
        for (int i = mid; i >= start; i--){
            leftSum += nums[i];
            if (leftSum > leftMidMax){
                leftMidMax = leftSum;
            }
        }

        int rightMidMax = nums[mid + 1];
        int rightSum = 0;
        for (int i = mid + 1; i <= end; i++){
            rightSum += nums[i];
            if(rightSum > rightMidMax){
                rightMidMax = rightSum;
            }
        }

        int midMax = leftMidMax + rightMidMax;

        if (lMax >= rMax && lMax >= midMax){
            return lMax;
        }
        else if (rMax >= lMax && rMax >= midMax){
            return rMax;
        }
        else{
            return midMax;
        }
    }
};
```

时间复杂度为$O(NlogN)$，空间复杂度为$O(logN)$
