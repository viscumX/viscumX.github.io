---
title: 剑指offer-数据流的中位数
date: 2021-05-27 10:43:26
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4]  的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

- void addNum(int num) - 从数据流中添加一个整数到数据结构中。
- double findMedian() - 返回目前所有元素的中位数。

**刷题链接**：[https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof)

<!--more-->

## 解法

### 暴力

```C++
class MedianFinder {
public:
    MedianFinder() {

    }

    void addNum(int num) {
        v.emplace_back(num);
    }

    double findMedian() {
        sort(v.begin(), v.end());

        int sz = v.size();
        return (double)(n & 1 ? v[sz>>1] : (v[sz>>1-1] + v[sz>>1]) / 2);
    }
private:
    vector<int> v;
};
```

时间复杂度约为$O(NlogN)$，空间复杂度为$O(N)$

### 插入排序

```C++
class MedianFinder {
public:
    MedianFinder() {

    }

    void addNum(int num) {
        if(v.empty()){
            v.emplace_back(num);
        }
        else{
            v.insert(lower_bound(v.begin(), v.end(), num), num);
        }
    }

    double findMedian() {
        int sz = v.size();
        return (double)(n & 1 ? v[sz>>1] : (v[sz>>1-1] + v[sz>>1]) / 2);
    }
private:
    vector<int> v;
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 优先队列

用一个最大堆和最小堆，并且平衡两个堆中数的数量

```C++
class MedianFinder {
public:
    MedianFinder() {

    }

    void addNum(int num) {
        low.push(num);
        high.push(low.top());
        low.pop();

        if(low.size()<high.size()){
            low.push(high.top());
            high.pop();
        }
    }

    double findMedian() {
        return (double)(low.size() > high.size() ? low.top() : (low.top()+high.top())/2);
    }
private:
    priority_queue<int> low;
    priority_queue<int, vector<int>, greater<int>> high;
};
```

时间复杂度为$O(logN)$，空间复杂度为$O(N)$

### Multiset 和双指针

```C++
class MedianFinder {
public:
    MedianFinder() {
        lowMedian = data.end();
        highMedian = data.end();
    }

    void addNum(int num) {
        const size_t n = data.size();
        data.insert(num);
        if(!n){
            lowMedian = highMedian = data.begin();
        }
        else if(n & 1){
            if(num < *lowMedian){
                lowMedian--;
            }
            else{
                highMedian++;
            }
        }
        else{
            if(num > *lowMedian && num < *highMedian){
                lowMedian++;
                highMedian--;
            }
            else if(num >= *highMeidan){
                lowMedian++;
            }
            else{
                lowMedian = --highMedian;
            }
        }
    }

    double findMedian() {
        return (*lowMedian + *highMedian) * 0.5;
    }
private:
    multiset<int> data;
    multiset<int>::iterator lowMedian, highMedian;
};
```

时间复杂度$O(logN)$，空间复杂度$O(N)$
