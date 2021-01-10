---
title: Leetcode刷题-电话号码的字母组合
date: 2021-01-04 10:17:13
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。数字到字母的映射与电话按键相同，1 不对应任何字母

**刷题链接**：[https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

<!--more-->

## 解法

这题的时间空间复杂度比较难分析，暂且不谈

### bfs 递归

用哈希表存储数字与字母间的对应关系。遍历数字串时，就是当前的数字对应的字符分别作为后缀与前面计算出的字符串相加

```C++
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        unordered_map<char, string> hashtable;
        hashtable['2'] = "abc";
        hashtable['3'] = "def";
        hashtable['4'] = "ghi";
        hashtable['5'] = "jkl";
        hashtable['6'] = "mno";
        hashtable['7'] = "pqrs";
        hashtable['8'] = "tuv";
        hashtable['9'] = "wxyz";
        ans = recursion(digits, ans, hashtable);
        return ans;
    }

    vector<string> recursion(string str, vector<string> strSet, unordered_map<char, string> hashtable) {
        if (!str.size()) {
            return strSet;
        }
        char first = str[0];
        int strSetSize = strSet.size();
        string s = hashtable[first];
        vector<string> temp(strSet);
        if (!strSetSize) {
            strSet.resize(s.size());
            for (int i = 0; i < strSet.size(); i++) {
                strSet[i] = s[i % (s.size())];
            }
        }
        else {
            strSet.resize(strSetSize*s.size());
            for (int i = 0; i < strSet.size(); i++) {
                strSet[i] = temp[i / (s.size())] + s[i % (s.size())];
            }
        }
        str = str.substr(1);
        return recursion(str, strSet, hashtable);
    }
};
```

### dfs 递归

先计算出每个结果的字符串，即 dfs，代码看起来比上面简单一点

```C++
class Solution {
public:
    vector<string> ans;
    unordered_map<char, string> hashtable;
    string temp;

    void recursion(string digits, int id) {
        if(id==digits.size()){
            ans.push_back(temp);
            return;
        }
        for(int i=0;i<hashtable[digits[id]].size();i++){
            temp.push_back(hashtable[digits[id]][i]);
            recursion(digits, id+1);
            temp.pop_back();
            }
        }
    vector<string> letterCombinations(string digits) {
        hashtable['2'] = "abc";
        hashtable['3'] = "def";
        hashtable['4'] = "ghi";
        hashtable['5'] = "jkl";
        hashtable['6'] = "mno";
        hashtable['7'] = "pqrs";
        hashtable['8'] = "tuv";
        hashtable['9'] = "wxyz";
        if(!digits.size()){
            return ans;
        }
        recursion(digits,0);
        return ans;
    }
};
```
