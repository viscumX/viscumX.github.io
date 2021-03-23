---
title: Leetcode刷题-只出现一次的数字
date: 2020-12-03 11:33:17
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

算法应该具有线性时间复杂度，尽量不使用额外空间来实现

**刷题链接**：[https://leetcode-cn.com/problems/single-number](https://leetcode-cn.com/problems/single-number)

<!--more-->

## 解法

### 哈希表

先遍历一遍数组，存出一个数字-出现次数的哈希表，然后遍历这个哈希表即可

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans;
        unordered_map<int,int> hashtable;
        for(int i=0;i<nums.size();i++){
            if(hashtable.find(nums[i])==hashtable.end()){
                hashtable[nums[i]]=0;
            }
            else{
                hashtable[nums[i]]=1;
            }
        }
        unordered_map<int,int>::iterator iter;
        for(iter=hashtable.begin();iter!=hashtable.end();iter++){
            if(!iter->second){
                ans = iter->first;
            }
        }
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 位运算

对数组的所有元素做异或运算，最后得到的结果就是只出现一次的数

```C++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans=0;
        for(int i=0;i<nums.size();i++){
            ans^=nums[i];
        }
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
