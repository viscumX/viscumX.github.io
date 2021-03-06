---
title: Leetcode刷题-下一个更大的元素（2）
date: 2021-03-06 17:46:37
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 x 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，循环地搜索它的下一个更大的数。如果不存在，则输出 -1。

**刷题链接**：[https://leetcode-cn.com/problems/next-greater-element-ii](https://leetcode-cn.com/problems/next-greater-element-ii)

<!--more-->

## 解法

### 暴力法

遍历给定数组，并将每个数组分为当前位置前和后依次搜索。注意数组中可能存在-1

```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> res;
        for(int i=0;i<nums.size();i++){
            int bigger = nums[i];
            for(int j=i;j<nums.size();j++){
                if(nums[j]>nums[i]){
                    bigger = nums[j];
                    break;
                }
            }
            if(bigger==nums[i]){
                for(int j=0;j<i;j++){
                    if(nums[j]>nums[i]){
                        bigger = nums[j];
                        break;
                    }
                }
            }
            if(bigger==nums[i]){
                bigger = -1;
            }
            res.emplace_back(bigger);
        }
        return res;
    }
};
```

时间复杂度为$O(N^2)$，空间复杂度为$O(1)$

### 单调栈

构建一个单调递减栈，存放单调递减数组元素的下标，对于栈中下标对应的数组元素，下一个更大的数是所有元素的更大数。对于循环数组而言，可以将数组展开，即将数组去尾，接在末尾上，然后取模来和原数组对应

```C++
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        if(nums.empty()){
            return nums;
        }
        vector<int> res(nums.size(), -1);
        stack<int> s;
        for(int i=0;i<nums.size()*2-1;i++){
            while(!s.empty()&&nums[s.top()]<nums[i%nums.size()]){
                res[s.top()] = nums[i%nums.size()];
                s.pop();
            }
            s.push(i%nums.size());
        }
        return res;
    }
}
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
