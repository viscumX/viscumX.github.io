---
title: 剑指offer刷题-复杂链表的复制
date: 2021-03-16 11:18:30
categories: 刷题记录
tags: 刷题
---

## 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针 random 指向一个随机节点），请对此链表进行深拷贝，并返回拷贝后的头结点。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

**刷题链接**：[https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

<!--more-->

## 解法

复杂链表相对于普通链表的不同在于它有一个随机指针`*random`，它的数据结构如下

```C++
typedef struct{
    int label;
    RandomListNode *next, *random;
    RandomListNode(int x) :
        label(x), next(NULL), random(NULL) { }
}RandomListNode;
```

## 哈希表

