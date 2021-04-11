---
title: 剑指offer-丑数
date: 2021-04-11 12:19:33
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

把只包含质因子 2、3 和 5 的数称作丑数。求按从小到大的顺序的第 n 个丑数。

**刷题链接**：[https://leetcode-cn.com/problems/chou-shu-lcof/](https://leetcode-cn.com/problems/chou-shu-lcof/)

<!--more-->

## 解法

### dp + 三指针

dp 大法好，加入 dp 中的元素为当前对应指针乘 2，乘 3，乘 5 的结果的最小值，注意去重

```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> dp(n+1);
        dp[1] = 1;
        int p2 = 1, p3 = 1, p5 = 1;
        for(int i = 2; i < n + 1; ++i){
            int num2 = dp[p2] * 2, num3 = dp[p3] * 3, num5 = dp[p5] * 5;
            dp[i] = min(min(num2, num3), num5);
            if(dp[i]==num2){
                ++p2;
            }
            if(dp[i]==num3){
                ++p3;
            }
            if(dp[i]==num5){
                ++p5;
            }
        }
        return dp[n];
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 优先队列（小顶堆）

优先队列 priority_queue 是基于堆实现的拥有排序功能的队列。同时要注意去重，去重加排序也可以考虑用 set 来实现

```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        if(!index){
            return 0;
        }
        priority_queue<long, vector<long>, greater<long> > heap;
        long res = 1;
        for(int i = 1; i < index; ++i){
            heap.push(res*2);
            heap.push(res*3);
            heap.push(res*5);
            res = heap.top();
            heap.pop();
            while(!heap.empty() && res == heap.top()){
                heap.pop();
            }
        }
        return res;
    }
};
```

得到第 N 个数需要 N 重循环，优先队列存元素的时间复杂度为$O(logN)$，取最小值的时间复杂度为$O(1)$，故总时间复杂度为$O(NlogN)$，空间复杂度为$O(N)$
