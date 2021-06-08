---
title: Leetcode-将有序数组转换为二叉搜索树
date: 2021-06-08 22:23:08
categories: 刷题记录
tags: [刷题, dfs, bts]
---

## 题目描述

一个整数数组 nums ，其中元素已经按升序排列，将其转换为一棵高度平衡二叉搜索树。

高度平衡二叉树是一棵满足**每个节点的左右两个子树的高度差的绝对值不超过 1**的二叉树。

**刷题链接**：[https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree)

<!--more-->

## 解法

了解中序遍历的特点就容易想到二分

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
    TreeNode* helper(vector<int> nums, int low, int high){
        if(low>high){
            return NULL;
        }
        int mid = low+((high-low)>>1);
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = helper(nums, low, mid-1);
        root->right = helper(nums, mid+1, high);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums, 0, nums.size()-1);
    }
};
```
