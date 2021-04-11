---
title: Leetcode-反转链表
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

**刷题链接**：[https://leetcode-cn.com/problems/reverse-linked-list/](https://leetcode-cn.com/problems/reverse-linked-list/)

<!--more-->

## 解法

### 迭代

只要在最开始设一个空节点，将当前节点的 next 都改为指向前一个节点

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

时间复杂度为$O(N)$，空间复杂度为$O(1)$

### 递归

假设某节点之后的所有节点都已经反转，

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
        if(!head||!head->next){
            return head;
        }
        ListNode* newHead = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return newHead;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
