---
title: Leetcode刷题-盛最多水的容器
date: 2020-12-28 19:47:25
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点  (i, ai) 。在坐标内画 n 条垂直线，垂直线 i  的两个端点分别为  (i, ai) 和 (i, 0) 。找出其中的两条线，使得它们与  x  轴共同构成的容器可以容纳最多的水。不能倾斜容器。

**刷题链接**：[https://leetcode-cn.com/problems/container-with-most-water](https://leetcode-cn.com/problems/container-with-most-water)

## 解法

### 双指针

从容器的两端开始向中间靠近，记录每次的容器容积，每次的最小值向内更新

```C++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size()-1;
        int shortLine;
        int res = 0;
        while(left<right){
            shortLine = min(height[left],height[right]);
            int resTemp = (right-left)*shortLine;
            res = res>resTemp?res:resTemp;
            if(height[left]>height[right]){
                right--;
            }
            else{
                left++;
            }
        }
        return res;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
