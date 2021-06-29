---
title: Leetcode-打开转盘锁
date: 2021-06-29 15:03:53
categories: 刷题记录
tags: 刷题
---

## 题目描述

有一个带有四个圆形拨轮的转盘锁。每个拨轮都有 10 个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为  '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1

**刷题链接**：[https://leetcode-cn.com/problems/open-the-lock](https://leetcode-cn.com/problems/open-the-lock)

<!--more-->

## 解法

### BFS

常规的 BFS 搜索，类似于层序遍历

```C++
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> s;  // 防止str重复
        queue<string> q;
        int cnt = 0;
        for (auto str: deadends) {
            s.emplace(str);
        }
        q.push("0000");
        while (!q.empty()) {
            int size = q.size();
            while (size--) {
                string cur = q.front();
                q.pop();
                if (cur == target) {
                    return cnt;
                }
                if (s.count(cur)) {
                    continue;
                }
                s.emplace(cur);
                for (int i = 0; i < 4; ++i) {
                    string tmp = cur;
                    tmp[i] = (cur[i] - '0' + 1) % 10 + '0';
                    q.push(tmp);
                    tmp[i] = (cur[i] - '0' - 1 + 10) % 10 + '0';
                    q.push(tmp);
                }
            }
            cnt++;
        }
        return -1;
    }
};
```

### 双向 BFS

可以大大减少空间复杂度

```C++
class Solution {
private:
    string t, s;
    unordered_set<string> set;
    int update(queue<string>& q, unordered_map<string, int>& m1, unordered_map<string, int>& m2) {
        string first = q.front();
        q.pop();
        int step = m1[first];
        for (int i = 0; i < 4; ++i) {   // 4位枚举-1和+1
            for (int j = -1; j <= 1; ++j) {
                if (j == 0) {
                    continue;
                }
                int origin = first[i] - '0';
                int next = (origin + j) % 10;
                if (next == -1) {
                    next = 9;
                }
                string clone = first;
                clone[i] = '0' + next;
                if (set.count(clone) || m1.find(clone) != m1.end()) {   // 已经遍历过了
                    continue;
                }
                if (m2.find(clone) != m2.end()) {
                    return step + 1 + m2[clone];
                } else {
                    q.push(clone);
                    m1[clone] = step + 1;
                }
            }
        }
        return -1;
    }
    int bfs() {
        queue<string> q1, q2;
        unordered_map<string, int> m1, m2;
        q1.push(s);
        m1[s] = 0;
        q2.push(t);
        m2[t] = 0;
        while (!q1.empty() && !q2.empty()) {  // 只要有一个队列空了，就说明没搜到
            int times = -1;
            if (q1.size() <= q2.size()) {  // 该搜q1了
                times = update(q1, m1, m2);
            } else {  // 该搜q2了
                times = update(q2, m2, m1);
            }
            if (times != -1) {  // 这次搜到了
                return times;
            }
        }
        return -1;
    }
public:
    int openLock(vector<string>& deadends, string target) {
        s = "0000";
        t = target;
        if (s == t) {
            return 0;
        }
        for (auto str: deadends) {
            set.insert(str);
        }
        if (set.count(s)) {
            return -1;
        }
        return bfs();
    }
};
```
