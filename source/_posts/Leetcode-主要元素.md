---
title: Leetcode-主要元素
date: 2021-07-09 10:35:36
categories: 刷题记录
tags: 刷题
---

## 题目描述

数组中占比超过一半的元素称之为主要元素。找出整数数组的主要元素，若没有，返回-1。请设计时间复杂度为`O(N)`、空间复杂度为`O(1)`的解决方案。

**刷题链接**：[https://leetcode-cn.com/problems/find-majority-element-lcci](https://leetcode-cn.com/problems/find-majority-element-lcci)

<!--more-->

## 解法

投票算法，两次扫描，记录候选数和计数。计数初始化为 0，下一个数不等于候选数计数就减一，否则加一。如果存在主要元素，则计数一定不为 0。如果不存在的话，计数也可能非 0，还需要进行第二次扫描确认候选数是否为主要元素

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candicate = 0;
        int count = 0;
        for (auto num : nums) {
            if (count == 0) {
                candicate = num;
            }
            if (candicate == num) {
                count += 1;
            } else {
                count -= 1;
            }
        }
        count = 0;
        for (auto num : nums) {
            if (num == candicate) {
                count += 1;
            }
        }
        return count * 2 > nums.size() ? candicate : -1;
    }
};
```
