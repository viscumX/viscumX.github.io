---
title: Leetcode刷题--两数之和
date: 2020-10-31 18:35:14
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个整数数组 nums  和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

刷题链接：[https://leetcode-cn.com/problems/two-sum/](https://leetcode-cn.com/problems/two-sum/)

<!--more-->

## 解法

### 暴力法

可以直接遍历两遍数组，算出所有的两数之和与 target 值进行比较

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ans(2,0);
        int nums_size = nums.size();
        for(int i=0;i<nums_size;i++){
            for(int j=i+1;j<nums_size;j++){
                if(nums[i]+nums[j]==target){
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }
        return ans;
    }
};
```

时间复杂度为 $O(N^2)$ ，是比较耗时的

### Hashmap 法

个人看来，哈希表就是建立 key 到 value 的一对一映射，通过建立哈希表快速寻找 target-x(当前值) 对应的下标

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        vector<int> ans(2,0);
        for(int i=0;i<nums.size();i++){
            if(hashtable.find(target-nums[i])!=hashtable.end()){
                ans[0] = hashtable[target-nums[i]];
                ans[1] = i;
                break;
            }
            hashtable[nums[i]] = i;
        }
        return ans;
    }
};
```

其中还用到了 unordered_map 容器，和 python 中的字典很像，find() 方法可以寻找到 map 中对应关键字的元素定位器，如果找不到则返回一个指向 end() 的定位器

unordered_map 采用哈希表实现 find() 的时间复杂度为 $O(1)$ ，所以整体复杂度为 $O(N)$
