---
title: 剑指offer-正则表达式匹配
date: 2021-05-25 13:56:58
categories: 刷题记录
tags: 刷题
---

## 题目描述

请实现一个函数用来匹配包含`'.'`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（含 0 次）。匹配是指字符串的所有字符匹配整个模式。例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但与`"aa.a"`和`"ab*a"`均不匹配。

**刷题链接**：[https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof](https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof)

<!--more-->

## 解法

动态规划，用 dp[i][j]表示 str 的前 i 个字符与 pattern 中的前 j 个字符是否能够匹配。

- 如果 pattern 的第 j 个字符是一个小写字母，就必须在 str 中匹配一个相同的小写字母
- 如果 pattern 的第 j 个字母是`*`，那么就表示可以对 pattern 的第 j-1 个字符匹配任意次
- 如果 pattern 的第 j 个字母是`.`，则必定匹配 str 中的一个字母

```C++
class Solution {
public:
    bool match(string str, string pattern) {
        int length1 = str.length();
        int length2 = pattern.length();
        vector<vector<int>> dp(length1+1,vector<int> (length2+1));
        dp[0][0]=1;

        for(int i=0;i<=length1;++i){
            for(int j=1;j<=length2;++j){
                if(pattern[j-1]=='*'){
                    dp[i][j] |= dp[i][j-2];
                    if(i!=0 && (pattern[j-2]=='.'||str[i-1]==pattern[j-2])){
                        dp[i][j] |= dp[i-1][j];
                    }
                }
                else{
                    if(i!=0 && (pattern[j-1]=='.' || str[i-1]==pattern[j-1])){
                        dp[i][j] |= dp[i-1][j-1];
                    }
                }
            }
        }
        return dp[length1][length2];
    }
};
```
