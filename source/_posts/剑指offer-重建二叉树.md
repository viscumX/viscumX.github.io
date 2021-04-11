---
title: 剑指offer-重建二叉树
date: 2021-01-13 16:29:10
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回

**刷题链接**：[https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

<!--more-->

## 解法

递归在二叉树中是十分常用的技巧。本题的解法涉及到用递归重建二叉树和遍历二叉树的方法。其实就是利用前序遍历中的根节点寻找中序遍历中的左右子节点部分，然后不断重复这个操作

```C++
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.size()==0||vin.size()==0){
            return NULL;
        }
        TreeNode* root = new TreeNode(pre[0]);

        int id = 0;
        while(id<vin.size()){
            if(root->val==vin[id]){
                break;
            }
            id++;
        }

        vector<int> preLeft, preRight, vinLeft, vinRight;
        for(int i=0;i<id;i++){
            preLeft.emplace_back(pre[i+1]);
            vinLeft.emplace_back(vin[i]);
        }
        for(int i=id+1;i<pre.size();i++){
            preRight.emplace_back(pre[i]);
            vinRight.emplace_back(vin[i]);
        }  // 也可以考虑用迭代器

        root->left = reConstructBinaryTree(preLeft, vinLeft);
        root->right = reConstructBinaryTree(preRight, vinRight);

        return root;
    }
};
```
