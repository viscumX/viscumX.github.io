---
title: Leetcode-验证二叉搜索树
date: 2021-06-07 18:08:02
categories: 刷题记录
tags: [dfs, 刷题]
---

## 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

- 节点的左子树只包含小于当前节点的数。
- 节点的右子树只包含大于当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**刷题链接**：[https://leetcode-cn.com/problems/validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree)

<!--more-->

## 解法

记录父节点的值

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
    bool helper(TreeNode* root, long long low, long long high){
        if(root==NULL){
            return true;
        }
        if(root->val <= low || root->val >= high){
            return false;
        }
        return helper(root->left, low, root->val) && helper(root->right, root->val, high);
    }
    bool isValidBST(TreeNode* root) {
        return helper(root, LONG_MIN, LONG_MAX);
    }
};
```
