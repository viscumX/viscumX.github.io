---
title: Leetcode刷题-反转链表
date: 2021-01-11 13:49:35
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

反转一个单链表。

例如：

```
input: 1->2->3->4->5->NULL
output: 5->4->3->2->1->NULL
```

<!--more-->

## 解法

### 迭代

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL){
            return NULL;
        }
        ListNode *rev = NULL;
        while(head!=NULL){
            ListNode* temp = head->next;
            head->next = rev;
            rev = head;
            head = temp;
        }
        return rev;
    }
};
```