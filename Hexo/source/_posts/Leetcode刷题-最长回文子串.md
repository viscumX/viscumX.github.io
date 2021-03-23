---
title: Leetcode刷题-最长回文子串
date: 2020-12-23 16:29:46
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000

**刷题链接**：[https://leetcode-cn.com/problems/longest-palindromic-substring/](https://leetcode-cn.com/problems/longest-palindromic-substring/)

<!--more-->

## 解法

### 中心扩散法

暴力法想想就不止$O(N^2)$所以直接就没考虑，遍历字符串的时候以当前字符 **s[i]** 为中心寻找回文串，需要考虑两种情况，即回文串长度为奇数和偶数

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        if(s==""){
            return s;
        }
        int maxLength = 1;
        int left=0;
        for(int i=0;i<s.size();i++){
            int lp=i;
            int rp=i;
            while(lp>-1&&rp<s.size()&&s[lp]==s[rp]){
                lp--;
                rp++;
            }
            if(rp-lp-1>maxLength){
                maxLength = rp-lp-1;
                left = lp+1;
            }
            lp=i;
            rp=i+1;
            while(lp>-1&&rp<s.size()&&s[lp]==s[rp]){
                lp--;
                rp++;
            }
            if(rp-lp-1>maxLength){
                maxLength = rp-lp-1;
                left = lp+1;
            }
        }
        return s.substr(left,maxLength);
    }
};
```

时间复杂度为$O(N^2)$，空间复杂度为$O(1)$

### 动态规划

构建一个二维数组用于存放 **[i,j]** 是否为回文串，由回文串的递推关系和边界条件来进行遍历

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n));
        string ans;
        for (int len = 0; len < n; len++) {
            for (int i = 0; i + len < n; i++) {
                int j = i + len;
                if (len == 0) {
                    dp[i][j] = 1;
                } else if (len == 1) {
                    dp[i][j] = (s[i] == s[j]);
                } else {
                    dp[i][j] = (s[i] == s[j] && dp[i + 1][j - 1]);
                }
                if (dp[i][j] && len + 1 > ans.size()) {
                    ans = s.substr(i, len + 1);
                }
            }
        }
        return ans;
    }
};
```

时间复杂度为$O(N^2)$，空间复杂度为$O(N^2)$

### Manacher 法

比较复杂的算法，不常用，这里只讲讲原理和思路

在字符两两之间加上未出现过的字符作为间隔，计算每个字符最多扩散多少步形成回文串，而这个最大步数就是最长回文串的长度

比如对于字符串"babad"，加上"#"后变为"#b#a#b#a#d"，设最大步数为 step

|      |  #  |  b  |  #  |  a  |  #  |  b  |  #  |  a  |  #  |  d  |  #  |
| :--: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|  i   |  0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |  9  | 10  |
| step |  0  |  1  |  0  |  3  |  0  |  3  |  0  |  1  |  0  |  1  |  0  |

则最长字符串为"bab"和"aba”，最长长度为 3
