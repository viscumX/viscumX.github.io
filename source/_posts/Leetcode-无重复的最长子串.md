---
title: Leetcode-无重复的最长子串
date: 2020-12-21 19:38:36
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个字符串，请找出其中不含有重复字符的最长子串的长度

- 0 <= s.length <= 5 \* 104
- s 由英文字母、数字、符号和空格组成

**刷题链接**：[https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

<!--more-->

## 解法

### 子串截取

遇到一样的字符就对存放的子串截取其后面的内容，再把遇到的字符放进来

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        string subString;
        int subSize = 0;
        for(auto alp: s){
            string::size_type id = subString.find(alp);
            if(id!=string::npos){
                subString = subString.substr(++id);
            }
            subString+=alp;
            subSize = subSize>subString.size()?subSize:subString.size();
        }
        return subSize;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 哈希表优化滑动窗口

思想和上一种方法差不多，但是用 unordered_set 来优化效率

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> hashtable;
        int maxStr = 0;
        int left = 0;
        for(int i = 0; i < s.size(); i++){
            while (hashtable.find(s[i]) != hashtable.end()){
                hashtable.erase(s[left]);
                left++;
            }
            maxStr = max(maxStr,i-left+1);
            hashtable.insert(s[i]);
    }
        return maxStr;
    }
};
```
