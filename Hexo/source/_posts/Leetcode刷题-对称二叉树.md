---
title: Leetcode刷题-对称二叉树
date: 2020-11-10 10:41:08
categories: 刷题记录
tags: 刷题
mathjax: true
---

## 题目描述

给定一个二叉树，检查它是否是镜像对称的

例如，二叉树 [1,2,2,3,4,4,3] 是对称的

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**刷题链接**：[https://leetcode-cn.com/problems/symmetric-tree/](https://leetcode-cn.com/problems/symmetric-tree/)

<!--more-->

## 解法

### 递归

检查一个二叉树是否镜像对称，那么其中某一节点的左子树值应该和对应节点的右子树值相等，右子树值应该和对应节点的左子树值相等

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
    bool isSymmetric(TreeNode* root) {
        return checkChildRoot(root, root);
    }

    bool checkChildRoot(TreeNode* child1, TreeNode* child2){
        if(!child1&&!child2){
            return true;
        }
        if(!child1||!child2){
            return false;
        }
        if(child1->val==child2->val){
            return checkChildRoot(child1->left, child2->right)&&checkChildRoot(child1->right, child2->left);
        }
        else{
            return false;
        }
    }
};
```

时间复杂度是$O(N)$，空间复杂度是$O(N)$

### 迭代

根节点两次入队，对应节点依次入队，对比的时候一同出队

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
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        q.push(root);
        while(!q.empty()){
            TreeNode* child1 = q.front();
            q.pop();
            TreeNode* child2 = q.front();
            q.pop();
            if (!child1&&!child2)
                continue;
            if (!child1||!child2)
                return false;
            if(child1->val!=child2->val)
                return false;
            q.push(child1->left);
            q.push(child2->right);
            q.push(child1->right);
            q.push(child2->left);
        }
        return true;
    }
};
```

时间复杂度为$O(N)$，空间复杂度为$O(N)$
