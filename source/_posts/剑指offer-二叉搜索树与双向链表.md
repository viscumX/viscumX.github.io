---
title: 剑指offer-二叉搜索树与双向链表
date: 2021-03-27 19:55:19
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

**刷题链接**：[https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

<!--more-->

## 解法

中序遍历二叉搜索树，并设置一个指向遍历到的节点的指针，调整节点之间的指向

```C++
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) { }
};*/
class Solution {
private:
    TreeNode* pNode = NULL;
    void inOrder(TreeNode* pRoot){
        if(!pRoot){
            return;
        }
        else{
            inOrder(pRoot->left);
            pRoot->left = pNode;
            if(pNode){
                pNode->right = pRoot;
            }
            pNode = pRoot;
            inOrder(pRoot->right);
        }
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree) {
        inOrder(pRootOfTree);
        while(pNode && pNode->left){
            pNode = pNode->left;
        }
        return pNode;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(1)$
