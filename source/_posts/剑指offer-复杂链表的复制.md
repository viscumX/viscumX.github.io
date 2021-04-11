---
title: 剑指offer-复杂链表的复制
date: 2021-03-16 11:18:30
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针 random 指向一个随机节点），请对此链表进行深拷贝，并返回拷贝后的头结点。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

**刷题链接**：[https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

<!--more-->

## 解法

复杂链表相对于普通链表的不同在于它有一个随机指针`*random`，它的数据结构如下

```C++
typedef struct RandomListNode{
    int label;
    RandomListNode *next, *random;
    RandomListNode(int x) :
        label(x), next(NULL), random(NULL) { }
}RandomListNode;
```

### 哈希表

第一次遍历先构造出所有的结点，并将旧结点和新节点的映射关系存成哈希表，并构造 next 关系。第二次遍历时在哈希表内查找 random 关系

```C++
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(!pHead){
            return NULL;
        }
        unordered_map<RandomListNode*, RandomListNode*> hashtable;
        RandomListNode* newHead = new RandomListNode(pHead->label);
        RandomListNode* p = pHead;
        RandomListNode* n = newHead;
        hashtable[pHead] = newHead;

        p = p->next;
        while(p){
            RandomListNode* temp = new RandomListNode(p->label);
            hashtable[p] = temp;
            n->next = temp;
            n = n->next;
            p = p->next;
        }

        p = pHead;
        n = newHead;
        while(p){
            if(p->random){
                n->random = hashtable[p->random];
            }
            else{
                n->random = NULL;
            }
            p = p->next;
            n = n->next;
        }

        return newHead;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$

### 复制结点

将链表的每个结点都复制一份，然后再拆开

```C++
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(!pHead){
            return NULL;
        }
        RandomListNode* newHead = pHead;
        while(pHead){
            RandomListNode* temp = new RandomListNode(pHead->label);
            temp->next = pHead->next;
            pHead->next = temp;
            pHead = temp->next;
        }
        pHead = newHead;
        while(pHead){
            RandomListNode* temp = pHead->next;
            if(pHead->random){
                temp->random = pHead->random->next;
            }
            pHead = temp->next;
        }
        //take apart
        pHead = newHead;
        RandomListNode* cloneHead = pHead->next;
        while(pHead->next){
            RandomListNode* next = pHead->next;
            pHead->next = next->next;
            pHead = next;
        }
        return cloneHead;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
