---
title: Leetcode刷题-环形链表
date: 2020-12-03 12:21:11
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。尽量用$O(1)$内存解决此问题

另外，链表中节点的数目范围是 [0, 104]

-105 <= Node.val <= 105

**刷题链接**：[https://leetcode-cn.com/problems/linked-list-cycle](https://leetcode-cn.com/problems/linked-list-cycle)

<!--more-->

## 解法

### 超界

非常暴力的解法，用 pos 记录节点数，随着链表节点一直增加，如果可以超过节点数就说明形成了一个环

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
    bool hasCycle(ListNode *head) {
        if(head==NULL)
            return false;
        int pos=-1;
        while(head->next!=NULL){
            head = head->next;
            pos++;
            if(pos>10000)
                return true;
        }
        return false;
    }
};
```

### 快慢指针

快指针每次走两步，慢指针每次走一步，如果相遇则说明有环

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
    bool hasCycle(ListNode *head) {
        if(head==NULL)
            return false;
        ListNode *slow = head;
        ListNode *fast = head;
        while(fast!=NULL&&fast->next!=NULL){
            slow = slow->next;
            fast = fast->next->next;
            if(slow==fast)
                return true;
        }
        return false;
    }
};
```

### 集合法

遍历链表时把链表放到集合里，如果碰到一样的元素就说明成环

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
    bool hasCycle(ListNode *head) {
        unordered_set<ListNode *> hashtable;
        while(head!=NULL){
            if(hashtable.find(head)!=hashtable.end()){
                return true;
            }
            else{
                hashtable.emplace(head);
                head = head->next;
            }
        }
        return false;
    }
};
```

这时候的空间复杂度就不是$O(1)$而是$O(N)$了

### 删除节点

令`node->next=node`，即节点指向自己就把这个节点从链表中删除了

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
    bool hasCycle(ListNode *head) {
        ListNode* nextNode;
        if(head==NULL||head->next==NULL){
            return false;
        }
        if(head==head->next){
            return true;
        }
        nextNode = head->next;
        head->next = head;
        return hasCycle(nextNode);
    }
};
```

### 反转链表

只要反转之后的链表的头节点指针和原链表的头节点指针相等，就说明肯定存在环，其实是把环整个删除了，剩下的节点再次反转

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
    ListNode* ReverseList(ListNode* head){
        ListNode* rev = NULL;
        while(head!=NULL){
            ListNode* temp = head->next;
            head->next = rev;
            rev = head;
            head = temp;
        }
        return rev;
    }
    bool hasCycle(ListNode *head) {
        ListNode* rev = ReverseList(head);
        if(head!=NULL&&head->next!=NULL&&rev==head){
            return true;
        }
        return false;
    }
};
```

空间复杂度为$O(1)$
