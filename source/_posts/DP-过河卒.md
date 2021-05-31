---
title: DP-过河卒
date: 2021-05-31 16:57:43
categories: 算法
tags: [动态规划, 搜索]
---

## 题目

字节夏令营第一场第二题和这题很类似。

**刷题链接**：[https://www.luogu.com.cn/problem/P1002](https://www.luogu.com.cn/problem/P1002)

<!--more-->

## 解法

马能到达的点和马本身所在的点是不能走的。对于卒的走法，达到下一个点只能是向右一格或向下一格，所以到达这一点的路径数就是它到达它左边一格或上边一个的点的路径数之和，由此得到递推关系。一个比较坑的点就是，用 int 的话结果可能会溢出

```C++
#include <iostream>
#include <vector>
using namespace std;

bool eat(int x, int y, int horsex, int horsey){
    return (horsex-2==x&&horsey-1==y)||(horsex-2==x&&horsey+1==y)
        ||(horsex-1==x&&horsey-2==y)||(horsex-1==x&&horsey+2==y)
        ||(horsex+1==x&&horsey-2==y)||(horsex+1==x&&horsey+2==y)
        ||(horsex+2==x&&horsey-1==y)||(horsex+2==x&&horsey+1==y)
        ||(horsex==x&&horsey==y);
}

int main(){
    int bx,by,horsex,horsey;
    cin >> bx >> by >> horsex >> horsey;
    vector<vector<long> > matrix(bx+1, vector<long>(by+1));
    for(int i=0;i<=by;++i){
        if(!eat(0,i,horsex,horsey)){
            matrix[0][i]=1;
        }else{
            break;
        }
    }
    for(int i=1;i<=bx;++i){
        if(!eat(i,0,horsex,horsey)){
            matrix[i][0]=1;
        }else{
            break;
        }
    }
    for(int i=1;i<=bx;++i){
        for(int j=1;j<=by;++j){
            if(eat(i,j,horsex,horsey)){
                matrix[i][j]=0;
            }
            else{
                matrix[i][j] = matrix[i-1][j]+matrix[i][j-1];
            }
        }
    }
    cout << matrix[bx][by];
    return 0;
}
```
