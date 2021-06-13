---
title: 剑指offer-剪绳子
date: 2021-05-28 23:06:59
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

**刷题链接**：[https://leetcode-cn.com/problems/integer-break/](https://leetcode-cn.com/problems/integer-break/)

<!--more-->

## 解法

### 动态规划

假设当前的数为 i，第一个拆分出的数为 j，那么就有两种拆分方案：

- 如果不拆分剩下的数，就是(i-j)\*j
- 如果拆分剩下的数，就是 dp[i-j]\*j

```C++
class Solution {
    public int integerBreak(int n) {
        vector <int> dp(n + 1);
        for (int i = 2; i <= n; i++) {
            int curMax = 0;
            for (int j = 1; j < i; j++) {
                curMax = max(curMax, max(j * (i - j), j * dp[i - j]));
            }
            dp[i] = curMax;
        }
        return dp[n];
    }
}
```

时间复杂度为$O(N^2)$，空间复杂度为$O(N)$

### 优化的动态规划

当 n = 2 时，最大乘积就是 1，当 i > 2 时，只用考虑 j = 2 或 j = 3 的情况

```C++
class Solution {
public:
    int integerBreak(int n) {
        if (n < 4) {
            return n - 1;
        }
        vector <int> dp(n + 1);
        dp[2] = 1;
        for (int i = 3; i <= n; i++) {
            dp[i] = max(max(2 * (i - 2), 2 * dp[i - 2]), max(3 * (i - 3), 3 * dp[i - 3]));
        }
        return dp[n];
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
