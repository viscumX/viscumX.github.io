---
title: Leetcode-有序链表转换二叉搜索树
date: 2021-06-09 13:59:43
categories: 刷题记录
tags: [刷题, 链表, BTS, dfs, 分治, 双指针]
---

## 题目描述

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点   的左右两个子树的高度差的绝对值不超过 1。

**刷题链接**：[https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree)

<!--more-->

## 解法

### 双指针分治

利用快慢指针求解链表中点

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    ListNode* getMedian(ListNode* left, ListNode* right){
        ListNode* fast = left;
        ListNode* slow = left;
        while(fast!=right&&fast->next!=right){
            fast = fast->next;
            fast = fast->next;
            slow = slow->next;
        }
        return slow;
    }
    TreeNode* helper(ListNode* left, ListNode* right){
        if(left==right){
            return NULL;
        }
        ListNode* mid = getMedian(left, right);
        TreeNode* root = new TreeNode(mid->val);
        root->left = helper(left, mid);
        root->right = helper(mid->next, right);
        return root;
    }
    TreeNode* sortedListToBST(ListNode* head) {
        return helper(head, NULL);
    }
};
```

### 中序遍历+分治

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
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
    int getLength(ListNode* head){
        int res = 0;
        while(head){
            head = head->next;
            res++;
        }
        return res;
    }
    TreeNode* helper(ListNode* &head, int left, int right){
        if(left>right){
            return NULL;
        }
        int mid = left+right+1>>1;
        TreeNode* root = new TreeNode();
        root->left = helper(head, left, mid-1);
        root->val = head->val;
        head = head->next;
        root->right = helper(head, mid+1, right);
        return root;
    }
    TreeNode* sortedListToBST(ListNode* head) {
        return helper(head, 0, getLength(head)-1);
    }
};
```
