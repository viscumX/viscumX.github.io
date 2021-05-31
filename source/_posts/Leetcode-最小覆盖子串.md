---
title: Leetcode-最小覆盖子串
date: 2021-05-31 11:50:11
categories: 刷题记录
tags: 刷题
---

## 题目描述

返回字符串 s 中涵盖字符串 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

注意：如果 s 中存在这样的子串，保证是唯一的答案。

**刷题链接**：[https://leetcode-cn.com/problems/minimum-window-substring](https://leetcode-cn.com/problems/minimum-window-substring)

<!--more-->

## 解法

滑动窗口，假设当前窗口中没有覆盖字串，就移动 r 指针，否则就移动 l 指针

```C++
class Solution {
public:
    string minWindow(string s, string t) {
        int sl = s.length();
        int minLen = INT_MAX;
        int l = 0, r = -1;
        int start = -1, stop = -1;
        unordered_map<char, int> sHash, tHash;
        for(auto ch: t){
            if(tHash.find(ch)!=tHash.end()){
                tHash[ch]++;
            }
            else{
                tHash[ch] = 1;
            }
        }
        while(r<sl){
            r++;
            if(tHash.find(s[r])!=tHash.end()){
                if(sHash.find(s[r])==sHash.end()){
                    sHash[s[r]] = 1;
                }
                else{
                    sHash[s[r]]++;
                }
            }
            while(l<=r){
                int flag = 0;
                for(auto it: tHash){
                    if(sHash[it.first]<it.second){
                        flag = 1;
                        break;
                    }
                }
                if(flag){
                    break;
                }
                if(minLen>r-l+1){
                    minLen = r-l+1;
                    start = l;
                    stop = r;
                }
                if(tHash.find(s[l])!=tHash.end()){
                    sHash[s[l]]--;
                }
                l++;
            }
        }
        return stop<0?"":s.substr(start, stop-start+1);
    }
};
```
