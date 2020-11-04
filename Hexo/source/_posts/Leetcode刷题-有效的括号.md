---
title: Leetcode刷题-有效的括号
date: 2020-11-03 10:22:40
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。

2. 左括号必须以正确的顺序闭合。

注意空字符串可被认为是有效字符串。
<!--more-->
---
## 解法
### 删除子串法
虽然一下想到了栈，但是没有深入的去考虑这种方法，而是选择了另一种比较复杂的方式。

输入的字符串长度一定是括号对数的两倍，删去所有括号对剩下的假设是有效的话一定是空串。

```C++
class Solution {
public:
    bool isValid(string s) {
        int slen = s.size();
        int shlen = slen/2;
        int id1,id2,id3;
        if (slen!=2*shlen){
            return false;
        }
        for (int i=0;i<=shlen;i++){
            id1 = s.find("()");
            id2 = s.find("[]");
            id3 = s.find("{}");
            if (id1!=string::npos){
                s.erase(id1,2);
            }
            else if(id2!=string::npos){
                s.erase(id2,2);
            }
            else if(id3!=string::npos){
                s.erase(id3,2);
            }
            else if(s.size()!=0){
                return false;
            }
        }
        return true;
    }
};
```
整体复杂度较高

### 栈法
主要的思想就是遇到左括号就压入栈内，匹配的右括号就出栈，遇上不匹配的右括号就说明不是有效的字符串，最后的栈是空的话就是可能有效的。不过还要考虑到一开始遇到的就是右括号的情况，这样也会使得最后的栈为空

```C++
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, int> m = {{'(',1}, {'[',2}, {'{',3}, {')',-1}, {']',-2}, {'}',-3}};
        stack<char> ch;
        bool istrue = 1;
        for (char c:s){
            if(m[c]>0&&m[c]<4){
                ch.push(c);
            }
            else if(!ch.empty()&&m[c]+m[ch.top()]==0){
                ch.pop();
            }
            else{
                istrue = false;
                break;
            }
        }
        if (!ch.empty()){
            istrue = false;
        }
        return istrue;
    }
};
```

只遍历了一遍字符串，时间复杂度为$O(N)$