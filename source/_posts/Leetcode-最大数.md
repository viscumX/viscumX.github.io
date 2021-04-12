---
title: Leetcode-最大数
date: 2021-04-12 23:02:56
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一组非负整数 nums，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

注意：输出结果可能非常大，所以你需要返回一个字符串而不是整数。

**刷题链接**：[https://leetcode-cn.com/problems/largest-number/](https://leetcode-cn.com/problems/largest-number/)

<!--more-->

## 解法

sort 函数可以进行自定义排序，对于 nums 的元素的大小比较其实就是转换为字符串之后，按不同顺序相加后大小的比较

```C++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        string res = "";
        sort(nums.begin(), nums.end(), [](const int &x, const int &y){
            string n1 = to_string(x);
            string n2 = to_string(y);
            return (n1 + n2) > (n2 + n1);
        });
        if(nums[0]==0){
            return "0";
        }
        for(auto item : nums){
            res += to_string(item);
        }
        return res;
    }
};
```

时间复杂度为$O(NlogN)$，其实就是排序算法的耗时，空间复杂度为$O(logN)$，也是排序算法所需的堆栈空间大小
