---
title: Leetcode刷题-不同的子序列
date: 2021-03-17 22:30:16
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。

字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE"  是  "ABCDE"  的一个子序列，而  "AEC"  不是）

题目数据保证答案符合 32 位带符号整数范围。

- 0 <= s.length, t.length <= 1000
- s 和 t 由英文字母组成

**刷题链接**：[https://leetcode-cn.com/problems/distinct-subsequences](https://leetcode-cn.com/problems/distinct-subsequences)

<!--more-->

## 解法

### dp 动态规划

假设 s 的第 j 个字符为 s[j]，t 的第 i 个字符为 t[i]。如果 s[j]和 t[i]相同，那么至 s[:j]中 t[:i]的个数为 s[:j-1]中 t[:i-1]的个数与 s[:j-1]中 t[:i]的个数之和，即选这个字符或者删除这个字符；如果 s[j]和 t[i]不同，那么至 s[:j]中 t[:i]的个数只能为 s[:j-1]中 t[:i]的个数，即必须删除这个数。由此得到状态方程

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        int sLength = s.length();
        int tLength = t.length();
        vector<vector<long long>> dp(tLength+1,vector<long long>(sLength+1,0));
        for(int i=0;i<sLength+1;i++){
            dp[0][i] = 1;
        }
        for(int i=1;i<tLength+1;i++){
            for(int j=1;j<sLength+1;j++){
                if(s[j-1] == t[i-1]){
                    dp[i][j] = dp[i-1][j-1]+dp[i][j-1];
                }
                else{
                    dp[i][j] = dp[i][j-1];
                }
            }
        }
        return dp[tLength][sLength];
    }
};
```

设两个字符串长度为 m 和 n，则时空复杂度均为$O(mn)$
