---
title: 剑指offer-二叉树的下一个结点
date: 2021-05-25 23:19:30
categories: 刷题记录
tags: 刷题
---

## 题目描述

给定一个二叉树其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的 next 指针。

<!--more-->

## 解法

分几种情况

```C++
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {

    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode) {
        if(!pNode){
            return pNode;
        }
        if(pNode->right){
            pNode = pNode->right;
            while(pNode->left){
                pNode = pNode->left;
            }
            return pNode;
        }
        while(pNode->next){
            TreeLinkNode *root = pNode->next;
            if(root->left == pNode){
                return root;
            }
            pNode = pNode->next;
        }
        return NULL;
    }
};
```
