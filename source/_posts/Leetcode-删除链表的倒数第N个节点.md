---
title: Leetcode-删除链表的倒数第N个节点
date: 2021-01-04 21:51:45
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点

**刷题链接**：[https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

<!--more-->

## 解法

### 反转链表

先将给定链表进行一次反转，然后删除对应位置的节点，删除节点的方法就是让它指向自己

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
    ListNode* reverseList(ListNode* head){
        if(head==NULL){
            return NULL;
        }
        ListNode* rev = NULL;
        while(head!=NULL){
            ListNode* temp = head->next;
            head->next = rev;
            rev = head;
            head = temp;
        }
        return rev;
    }
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* rev = reverseList(head);
        ListNode* frontNode = rev;
        ListNode* backNode;
        if(n==1){
            backNode = frontNode->next;
            frontNode->next = frontNode;
            rev = backNode;
        }
        else{
            for(int i=0;i<n-2;i++){
                frontNode = frontNode->next;
            }
            ListNode* delNode = frontNode->next;
            backNode = delNode->next;
            delNode->next = delNode;
            frontNode->next = backNode;
        }
        return reverseList(rev);
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$

### 计算链表长度

先计算链表的长度 L，再删去第(L-n+1)个节点

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(!head){
            return NULL;
        }
        ListNode *newHead = new ListNode(0, head);
        int ListLength = 0;
        while(head!=NULL){
            head = head->next;
            ++ListLength;
        }
        ListNode* frontNode = newHead;
        for(int i=1;i<ListLength-n+1;i++){
            frontNode = frontNode->next;
        }
        frontNode->next = frontNode->next->next;
        ListNode* ans = newHead->next;
        delete newHead;
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$

### 快慢指针

让两个指针相隔 n，当前一个指针指向 NULL 的时候，后一个指针就将指向需要删去的节点

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* newHead = new ListNode(0, head);
        ListNode* first = newHead;
        ListNode* second = newHead;

        for(int i=0;i<n+1;i++){
            first = first->next;
        }
        while(first){
            first = first->next;
            second = second->next;
        }
        second->next = second->next->next;
        ListNode* ans = newHead->next;
        delete newHead;
        return ans;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$，还有一种用栈的方法，就不写解法了，思想很简单
