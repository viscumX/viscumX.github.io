---
title: Leetcode刷题-二叉树的最大深度
date: 2020-11-10 11:39:51
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

**刷题链接**：[https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

<!--more-->

## 解法

### 递归/DFS

二叉树的问题好像很容易用到递归

最大深度就是（子树节点作为根节点的最大深度+1），这样就转换成了一个递归的问题

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
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (!root)
            return 0;
        int maxLd = maxDepth(root->left);
        int maxRd = maxDepth(root->right);
        int ans = maxLd>maxRd?maxLd:maxRd;
        return ans+1;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(height)$

### 迭代/BFS

就是一层一层地遍历，用队列来实现

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
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root)
            return 0;
        int depth = 0;
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* node;
        int n;
        while(!q.empty()){
            n = q.size(); //每层节点数
            while(n){
                node = q.front();
                if(node->left)
                    q.push(node->left);
                if(node->right)
                    q.push(node->right);
                q.pop();
                n -= 1;
            }
            depth += 1;
        }
        return depth;
    }
};
```

时间复杂度为$O(N)$，空间复杂度最大为$O(N)$
