---
title: Leetcode-删除排序列表中的重复元素（2）
date: 2021-03-25 22:58:22
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除链表中所有存在数字重复情况的节点，只保留原始链表中没有重复出现的数字。

要求返回同样按升序排列的结果链表。

**刷题链接**：[https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii)

<!--more-->

## 解法

### 指针迭代

用两个指针来界定要删除的长度

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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* pHead = new ListNode(-1);
        pHead->next = head;
        head = pHead;
        ListNode* left, *right;
        while(pHead->next){
            left = pHead->next;
            right = left;
            while(right->next && right->next->val == left->val){
                right = right->next;
            }
            if(left == right){
                pHead = pHead->next;
            }
            else{
                pHead->next = right->next;
            }
        }
        return head->next;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$。感觉换成递归也能做

### 设置标记

假设下一个节点值和当前节点值一致，则翻转标志位，若不一致则判断标志位是否翻转，如果未翻转了说明是不重复的节点，直接跳过，如果翻转了，说明下一个节点有可能是不重复的节点。对于最后一个节点，同样的假如标志位翻转了，说明是重复的节点，则末尾直接指向 NULL，假如没有翻转，就直接指向最后一个节点

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
    ListNode* deleteDuplicates(ListNode* head) {
        if(!head){
            return NULL;
        }
        ListNode* newHead = new ListNode(-1);
        newHead->next = head;
        ListNode* pre = newHead;
        ListNode* cur = head;
        int flag = 0;
        while(cur->next){
            if(cur->next->val == cur->val){
                cur = cur->next;
                flag = 1;
            }
            else{
                if(flag){
                    pre->next = cur->next;
                    flag = 0;
                }
                else{
                    pre = pre->next;
                }
                cur = cur->next;
            }
        }
        if(flag){
            pre->next = NULL;
        }
        return newHead->next;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
