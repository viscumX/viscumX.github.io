---
title: Leetcode刷题-三数之和
date: 2020-12-28 21:37:24
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c，使得 a + b + c = 0 ？请找出所有满足条件且不重复的三元组。答案中不可以包含重复的三元组。

**刷题链接**：[https://leetcode-cn.com/problems/3sum](https://leetcode-cn.com/problems/3sum)

<!--more-->

## 解法

### 暴力+两数和

具体做法就是将这个问题转化为两数之和的简单问题，但是又要求不重复，所以要加入排序和查找等方法

```C++
class Solution {
public:
	vector<vector<int>> threeSum(vector<int>& nums) {
		vector<vector<int>> sumSet;
		vector<int> sum(3);
		sort(nums.begin(), nums.end());
		for (int i = 0; i < nums.size();i++) {
			int twoSum = 0 - nums[i];
			unordered_map<int, int> hashtable;
			for (int j = i+1; j < nums.size(); j++) {
				if (hashtable.find(twoSum - nums[j]) != hashtable.end()) {
					sum[0] = nums[i];
					sum[1] = twoSum - nums[j];
					sum[2] = nums[j];
					if (find(sumSet.begin(), sumSet.end(), sum) == sumSet.end()) {
						sumSet.emplace_back(sum);
					}
				}
				hashtable[nums[j]] = twoSum - nums[j];
			}
		}
		return sumSet;
	}
};
```

时间复杂度最坏为$O(N^3)$，空间复杂度最坏为$O(N)$，find()函数的效率并不高，其时间复杂度最坏为$O(N)$

### 排序+双指针

先对给定数组进行排序，然后用双指针从左右两边向中间搜寻，中间要考虑去重，保证三个数不会使用重复的元素

```C++
class Solution {
public:
	vector<vector<int>> threeSum(vector<int>& nums) {
		vector<vector<int>> sumSet;
        sort(nums.begin(),nums.end());
        for(int i=0;i<nums.size();i++){
            if(i>0&&nums[i-1]==nums[i]){
                continue;
            }
            int right = nums.size()-1;
            for(int j=i+1;j<nums.size();j++){
                if(j>i+1&&nums[j-1]==nums[j]){
                    continue;
                }
                while(right>j&&nums[j]+nums[right]>-nums[i]){
                    right--;
                }
                if(j==right){
                    break;
                }
                if(nums[j]+nums[right]==-nums[i]){
                    sumSet.push_back({nums[i],nums[j],nums[right]});
                }
            }
        }
		return sumSet;
	}
};
```

时间复杂度为$O(N^2)$，空间复杂度为$O(logN)$，排序时使用的时间复杂度为$O(NlogN)$，空间复杂度为$O(logN)$
