---
title: Leetcode刷题-买卖股票的最佳时机
date: 2020-11-10 17:10:40
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。

注意：你不能在买入股票前卖出股票

**刷题链接**：[https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

<!--more-->

## 解法

### 暴力

遍历整个数组，算出所有的差

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans = 0;
        int days = prices.size();
        int diff;
        for (int i = 0;i<days;i++){
            for(int j = i;j<days;j++){
                diff = prices[j]-prices[i];
                ans = diff>ans?diff:ans;
            }
        }
        return ans;
    }
};
```

又超时了，时间复杂度为$O(N^2)$，空间复杂度为$O(1)$

### 动态规划

记录当前的最小价格，假如当天股价比最小价格高的话，就记录当前最大的收益

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice = INT_MAX;
        int ans = 0;
        for (int i = 0;i<prices.size();i++){
            if (minPrice>prices[i]){
                minPrice = prices[i];
            }
            else if (prices[i]-minPrice>ans){
                ans = prices[i]-minPrice;
            }
        }
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$

### 单调栈

单调栈就是栈中的元素严格递减或者严格递增

对于这题而言需要一个单调递增栈，每次弹出栈的时候与栈底的元素作差，最后一个入栈的元素为最小值（这里设为 0），动态更新最大的差，最后的结果即为最大收益

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        prices.emplace_back(-1);
        vector<int> increase_stack;
        int ans=0;
        for(int i=0;i<prices.size();i++){
            while(!increase_stack.empty()&&prices[i]<increase_stack.back()){
                if(increase_stack.back()-increase_stack.front()>ans){
                    ans = increase_stack.back()-increase_stack.front();
                }
                increase_stack.pop_back();
            }
            increase_stack.emplace_back(prices[i]);
        }
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
