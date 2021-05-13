---
title: 剑指offer-数组中的逆序对
date: 2021-04-14 20:38:02
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数

**刷题链接**：[https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/](https://leetcode-cn.com/problems/shu-zu-zhong-de-ni-xu-dui-lcof/)

<!--more-->

## 解法

### 归并排序

对数组进行归并排序，并在排序的过程中计算逆序对的个数

```C++
class Solution {
public:
    int mergeSort(vector<int>& nums, vector<int> &help, int l, int r){
        if(l>=r){
            return 0;
        }
        int mid = (l+r)/2;
        int invCount = mergeSort(nums, help, l, mid)+mergeSort(nums, help, mid+1, r);
        int i = l, j = mid+1;
        for(int k = l; k<=r; ++k){
            help[k] = nums[k];
        }

        for(int k = l;k<=r;++k){
            if(i==mid+1){
                nums[k] = help[j++];
            }
            else if(j==r+1||help[i]<=help[j]){
                nums[k] = help[i++];
            }
            else{
                nums[k] = help[j++];
                invCount += mid-i+1;
            }
        }
        return invCount;
    }

    int InversePairs(vector<int> data) {
        vector<int> help(data.size());
        return mergeSort(data, help, 0, data.size()-1);
    }
};
```

时间复杂度为$O(NlogN)$，空间复杂度为$O(N)$

### 树状数组

关于树状数组的介绍参考[https://blog.csdn.net/Yaokai_AssultMaster/article/details/79492190](https://blog.csdn.net/Yaokai_AssultMaster/article/details/79492190)

树状数组是一种用于高效处理对一个存储数字的列表进行更新及求前缀和的数据结构。构建 Binary Indexed Tree 的时间复杂度为 O(nlogn)或者 O(n)，取决于我们使用哪种算法

```C++
class BIT {
private:
    vector<int> tree;
    int n;

public:
    BIT(int _n): n(_n), tree(_n + 1) {}

    static int lowbit(int x) {
        return x & (-x);
    }

    int query(int x) {
        int ret = 0;
        while (x) {
            ret += tree[x];
            x -= lowbit(x);
        }
        return ret;
    }

    void update(int x) {
        while (x <= n) {
            ++tree[x];
            x += lowbit(x);
        }
    }
};

class Solution {
public:
    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        vector<int> tmp = nums;

        sort(tmp.begin(), tmp.end());
        for (int& num: nums) {
            num = lower_bound(tmp.begin(), tmp.end(), num) - tmp.begin() + 1;
        }

        BIT bit(n);
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            ans += bit.query(nums[i] - 1);
            bit.update(nums[i]);
        }
        return ans;
    }
};
```

sort()的时间复杂度为$O(logN)$，lower_bound()以二分查找为模板，时间复杂度为$O(logN)$，query()和 update()的时间复杂度也为$O(logN)$，故总时间复杂度为$O(NlogN)$。空间复杂度为树状数组需要的存储空间即$O(N)$
