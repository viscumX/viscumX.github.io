---
title: 剑指offer-字符串的排列
date: 2021-04-01 14:36:35
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

输入一个字符串，打印出该字符串中字符的所有排列。可以以任意顺序返回这个字符串数组，但里面不能有重复元素

**刷题链接**：[https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

<!--more-->

## 解法

### 递归交换元素位置

每次固定一个字符在第一位，依次交换后面某两位。用集合的特性来去除重复元素

```C++
class Solution {
public:
    void dfs(string str, int pos, set<string> &ans){
        if(pos == str.length()){
            ans.insert(str);
            return;
        }
        else{
            for(int i=pos;i<str.length();i++){
                swap(str[i], str[pos]);
                dfs(str, pos+1, ans);
                swap(str[i], str[pos]);
            }
        }
    }
    vector<string> Permutation(string str) {
        set<string> ans;
        dfs(str, 0, ans);
        vector<string> res = {ans.begin(), ans.end()};
        return res;
    }
};
```

时间复杂度为$O(N*N!)$，空间复杂度为$O(N^2)$

### 回溯布尔选择数组

用 set 来去重，用布尔数组来判断数组是否选中

```C++
class Solution {
public:
    set<string> ans;
    void dfs(string& s, string& cur, vector<bool>& used){
        if(cur.size() == s.size()){
            ans.insert(cur);
            return;
        }  // cur完整了
        for(int i = 0; i < s.size(); ++i){
            if(used[i]){
                continue;  // 使用过的就跳过
            }
            cur += s[i];
            used[i] = true;
            dfs(s, cur, used);
            used[i] = false;  // 回溯完毕，复原
            cur.pop_back();
        }
    }
    vector<string> Permutation(string s) {
        string cur = "";
        vector<bool> used(s.size(), false);
        dfs(s, cur, used);
        vector<string> res = {ans.begin(), ans.end()};
        return res;
    }
};
```
