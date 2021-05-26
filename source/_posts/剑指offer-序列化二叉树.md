---
title: 剑指offer-序列化二叉树
date: 2021-05-26 18:04:39
categories: 刷题记录
tags: 刷题
---

## 题目描述

请实现两个函数，分别用来序列化和反序列化二叉树。

**刷题链接**：[https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

<!--more-->

## 解法

### 层序遍历

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
        string s;
        TreeNode* pNode;
        queue<TreeNode*> q;
        q.push(root);
        while(!q.empty()){
            pNode = q.front();
            q.pop();
            if(!pNode){
                s += "null,";
                continue;
            }
            s += to_string(pNode->val)+",";
            q.push(pNode->left);
            q.push(pNode->right);
        }
        if(!s.empty()){
            s.pop_back();
        }
        return s;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data.empty()||data=="null"){
            return NULL;
        }
        vector<string> v;
        int i = 0, j = 0;
        while(j<data.length()){
            if(data[j]!=','){
                ++j;
            }
            else{
                string tmp = data.substr(i,j-i);
                v.emplace_back(tmp);
                ++j;
                i = j;
            }
        }
        if(i!=j){
            string tmp = data.substr(i);
            v.emplace_back(tmp);
        }
        queue<TreeNode*> q;
        TreeNode* res = new TreeNode(stoi(v[0]));
        q.push(res);
        i = 1;
        while(!q.empty()){
            TreeNode* pNode = q.front();
            q.pop();
            if(v[i]!="null"){
                pNode->left = new TreeNode(stoi(v[i]));
                q.push(pNode->left);
            }
            ++i;
            if(v[i]!="null"){
                pNode->right = new TreeNode(stoi(v[i]));
                q.push(pNode->right);
            }
            ++i;
        }
        return res;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```

### 先序遍历

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
    void dfs1(TreeNode* root, string& res){
        if(!root){
            res += "null,";
            return;
        }
        res += to_string(root->val)+",";
        dfs1(root->left, res);
        dfs1(root->right, res);
    }

    string serialize(TreeNode* root) {
        string s;
        dfs1(root, s);
        return s;
    }

    // Decodes your encoded data to tree.
    TreeNode* dfs2(string &data, int& u){
        if(data[u]=='n'){
            u += 5;
            return NULL;
        }
        int t = 0;
        bool flag = false;
        while(data[u] != ','){
            if(data[u] == '-'){
                flag = true;
            }
            else{
                t = t*10+(data[u]-'0');
            }
            ++u;
        }
        ++u;
        if(flag){
            t = -t;
        }

        TreeNode* root = new TreeNode(t);
        root->left = dfs2(data, u);
        root->right = dfs2(data, u);

        return root;
    }
    TreeNode* deserialize(string data) {
        return dfs2(data, 0);
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.deserialize(codec.serialize(root));
```
