---
title: 剑指offer-序列化二叉树
date: 2021-05-26 18:04:39
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

请实现两个函数，分别用来序列化和反序列化二叉树。

**刷题链接**：[https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

<!--more-->

## 解法

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string res = "";
        if(!root){
            return "[]";
        }
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* pNode;
        while(!q.empty()){
            pNode = q.front();
            if(pNode->left){
                q.push(pNode->left);
            }
            if(pNode->right){
                q.push(pNode->right);
            }
        }
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {

    }
};
```
