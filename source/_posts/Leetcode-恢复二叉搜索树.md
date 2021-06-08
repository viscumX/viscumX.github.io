---
title: Leetcode-恢复二叉搜索树
date: 2021-06-08 12:42:23
categories: 刷题记录
tags: [刷题, dfs, 二叉搜索树]
---

## 题目描述

已知二叉搜索树的根节点 root ，该树中的两个节点被错误地交换。请在不改变其结构的情况下，恢复这棵树。

**刷题链接**：[https://leetcode-cn.com/problems/recover-binary-search-tree](https://leetcode-cn.com/problems/recover-binary-search-tree)

<!--more-->

## 解法

记录两个乱序的结点位置

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode *t1, *t2, *prev;
    void inOrder(TreeNode* root){
        if(root==NULL){
            return;
        }
        inOrder(root->left);
        if(prev&&root->val<prev->val){
            t1 = root;
            if(t2==NULL){
                t2 = prev;
            }
        }
        prev = root;
        inOrder(root->right);
    }
    void recoverTree(TreeNode* root) {
        t2 = NULL;
        prev = NULL;
        inOrder(root);
        int tmp = t1->val;
        t1->val = t2->val;
        t2->val = tmp;
    }
};
```
