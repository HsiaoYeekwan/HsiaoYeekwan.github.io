---
title: Codeforces Round #500 (Div. 2) E.Hills (DP)
date: 2018-08-08 20:01:50
tags: [dp,codeforces]
category: ACM
---
## 题目链接
[codeforces 1013E](http://codeforces.com/contest/1013/problem/E)

## 题意
>给你一个长度为n的序列，当a[i] > a[i-1] && a[i] > a[i+1]时a[i] 为山峰
每次操作可以使任意一个数-1，问最少需要多少次操作可以使得恰好有k个山峰
对于![](http://codeforces.com/predownloaded/68/fe/68fee4f6670f7958bab3908d3d50ed653026b335.png)输出每个答案

<!--more-->
## 思路
>因为每个数只影响其相邻的数，很明显的dp。
定义dp[i][j][0/1]为前i个数选择j个山峰时,第i个数选/不选的最优值
还需要定义一个h[i][j][0/1]为前i个数选择j个山峰时,第i个数选/不选时第i个数的最大值(因为要保证答案最小)，因为我们使第i个数为山峰的时候除了前一个数还会影响后一个数。
状态转移为:
dp[i][j][0] = max(dp[i-1][j][1] + max(0 , a[i] - h[i-1][j][1] + 1) , dp[i-1][j][0])
dp[i][j][1] = dp[i-1][j-1][0] + max(0 , h[i-1][j-1][0] - a[i] + 1);
更新的同时也要更新h数组

```c
#include <iostream>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
#include <time.h>
using namespace std;
typedef long long ll;
const int maxn = 5000+10;
ll dp[maxn][maxn/2][2] , h[maxn][maxn/2][2];
ll a[maxn];
int n;
inline int readint()
{
    int res = 0, flag = 0;
    char ch;
    if((ch = getchar()) == '-')
        flag = 1;
    else if(ch >= '0' && ch <= '9')
        res = ch - '0';
    while((ch = getchar()) >= '0' && ch <= '9' )
        res = res * 10 + ch - '0';
    return flag ? -res : res;
}
int main()
{
    cin >> n;
    for(int i = 1 ; i <= n ; i ++){
        scanf("%d" , &a[i]);
    }
    for(int i = 0 ; i < maxn ; i ++){
        for(int j = 0 ; j <maxn / 2 ; j ++){
            dp[i][j][0] = dp[i][j][1] = 10000000000000000;
        }
        dp[i][0][0] = 0;
        h[i][0][0] = a[i];
    }
    for(int i = 1 ; i <= n ; i ++){
        for(int j = 1 ; j <= (i+1)/2 ; j ++){
            int p1 = dp[i-1][j][0];
            int p2 = dp[i-1][j][1] + max(0ll , a[i] - h[i-1][j][1] + 1);
            if(p1 < p2){
                dp[i][j][0] = p1;
                h[i][j][0] = a[i];
            }
            else{
                dp[i][j][0] = p2;
                h[i][j][0] = min(a[i] , h[i-1][j][1] - 1);
            }
            dp[i][j][1] = dp[i-1][j-1][0] + max(0ll , h[i-1][j-1][0] - a[i] + 1);
            h[i][j][1] = a[i];
        }
    }
    for(int i = 1 ; i <= (n+1)/2 ; i ++){
        printf("%lld%c" , min(dp[n][i][0] , dp[n][i][1]) , i == (n+1)/2 ? '\n' : ' ');
    }
    return 0;
}
```