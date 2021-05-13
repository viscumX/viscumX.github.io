---
title: Leetcode-扰乱字符串
date: 2021-04-16 12:58:05
categories:
tags:
---

## 题目描述

使用下面描述的算法可以扰乱字符串 s 得到字符串 t ：

1. 如果字符串的长度为 1 ，算法停止
2. 如果字符串的长度 > 1 ，执行下述步骤：
   - 在一个随机下标处将字符串分割成两个非空的子字符串。即，如果已知字符串 s ，则可以将其分成两个子字符串 x 和 y ，且满足 s = x + y 。
   - **随机**决定是要**交换两个子字符串**还是要**保持这两个子字符串的顺序不变**。即，在执行这一步骤之后，s 可能是 s = x + y 或者 s = y + x 。
   - 在 x 和 y 这两个子字符串上继续从步骤 1 开始递归执行此算法。

给你两个长度相等的字符串 s1 和 s2，判断 s2 是否是 s1 的扰乱字符串。如果是，返回 true；否则，返回 false 。

**刷题链接**：[https://leetcode-cn.com/problems/scramble-string/](https://leetcode-cn.com/problems/scramble-string/)

<!--more-->

## 解法

`dp[i][j][k]`表示 s2 从 j 开始的长度为 k 的区间是 s1 从 i 开始的长度为 k 的区间的扰乱字符串。

当 k 为 1 时，只要判断字符是否相等就行。当 k 大于 1，只要判断是否 s2 的前半部分是 s1 的前半部分的扰乱字符串&&s2 的后半部分是 s1 的后半部分的扰乱字符串或 s2 的后半部分是 s1 的前半部分的扰乱字符串&&s2 的前半部分是 s1 的后半部分的扰乱字符串

### 动态规划

```C++
class Solution {
public:
    bool isScramble(string s1, string s2) {
        if(s1==s2){
            return true;
        }
        if (s1.length() != s2.length()){
            return false;
        }
        int n = s1.length();
        vector<vector<vector<int> > > dp(n,vector<vector<int> > (n, vector<int> (n+1)));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                dp[i][j][1] = s1[i] == s2[j];
            }
        }
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                for (int j = 0; j <= n - len; j++) {
                    for (int k = 1; k < len; k++) {
                        int a = dp[i][j][k] && dp[i + k][j + k][len - k];
                        int b = dp[i][j + len - k][k] && dp[i + k][j][len - k];
                        if (a || b) {
                            dp[i][j][len] = 1;
                        }
                    }
                }
            }
        }
        return dp[0][0][n];
    }
};
```

时间复杂度为$O(N^4)$，空间复杂度为$O(N^3)$
