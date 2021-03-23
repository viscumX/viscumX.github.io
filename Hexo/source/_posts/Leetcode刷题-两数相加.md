---
title: Leetcode刷题-两数相加
date: 2020-12-17 17:49:01
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照逆序的方式存储的，并且它们的每个节点只能存储一位数字。

如果将这两个数相加起来，则会返回一个新的链表来表示它们的和。可以假设除了 0 之外，这两个数都不会以 0 开头。

**刷题链接**：[https://leetcode-cn.com/problems/add-two-numbers](https://leetcode-cn.com/problems/add-two-numbers)

<!--more-->

## 解法

### 遍历链表

基本模拟了两数相加的过程，遍历两个链表，其中较短的进行补 0。也可以考虑将将较短的链表直接接到答案链表上

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode();
        ListNode* ans = head;
        int super = 0;
        while(l1||l2||super){
            if(l1){
                super+=l1->val;
            }
            if(l2){
                super+=l2->val;
            }
            ans->val = super%10;
            super /= 10;
            if(l1->next!=NULL||l2->next!=NULL||super!=0){
                ans->next = new ListNode();
                ans = ans->next;
                l1 = l1->next;
                l2 = l2->next;
                if(!l1){
                    l1 = new ListNode(0);
                }
                if(!l2){
                    l2 = new ListNode(0);
                }
            }
            else{
                break;
            }
        }
        return head;
    }
};
```

时间复杂度为$O(max(m,n))$，m，n 分别为两个链表的长度，空间复杂度也是$O(max(m,n))$

### 递归

思想其实和上一种很相似，只不过换成递归的形式

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        return dfs(l1, l2, 0);
    }

    ListNode* dfs(ListNode* l1, ListNode* l2, int super) {
        if(!l1 && !l2 && !super){
            return NULL;
        }
        int sum = (l1?l1->val:0)+(l2?l2->val:0)+super;
        ListNode* node = new ListNode(sum%10);
        node->next = dfs(l1?l1->next:NULL, l2?l2->next:NULL, sum/10);
        return node;
    }
};
```

时空复杂度与上一种方法差不多
