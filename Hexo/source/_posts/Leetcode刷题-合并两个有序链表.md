---
title: Leetcode刷题-合并两个有序链表
date: 2020-11-06 13:23:20
categories:
tags:
mathjax: true
---

## 题目描述
将两个升序链表合并为一个新的**升序**链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的

刷题链接：[https://leetcode-cn.com/problems/merge-two-sorted-lists/](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

<!--more-->

## 解法

### 比较两个链表
比较容易想到的解法就是分别遍历两个链表，比较节点的大小。最后剩下的节点也一定是升序排列的比前面节点大的，所以直接指向剩下的节点就行

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode();
        ListNode* prev = head;
        while(l1!=nullptr&&l2!=nullptr){
            if(l1->val<l2->val){
                prev->next = l1;
                l1 = l1->next;
            }
            else{
                prev->next = l2;
                l2 = l2->next;
            }
            prev = prev->next;
        }
        if(l1==nullptr){
            prev->next = l2;
        }
        else{
            prev->next = l1;
        }
        return head->next;
    }
};
```
时间复杂度最多为$O(m+n)$，m 和 n 分别为两个链表的长度，空间复杂度为$O(1)$

### 递归法
递归这个东西真的让人又爱又恨

这就是两个链表头部较小的值与剩下的值合并的递归过程

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 == nullptr) {
            return l2;
        }
        else if (l2 == nullptr) {
            return l1;
        }
        else if (l1->val < l2->val) {
            l1->next = mergeTwoLists(l1->next, l2);
            return l1;
        }
        else {
            l2->next = mergeTwoLists(l1, l2->next);
            return l2;
        }
    }
};
```
时间复杂度为$O(m+n)$，m 和 n 分别为两个链表的长度，空间复杂度为$O(m+n)$，递归调用函数会占用栈空间并且取决于递归的次数