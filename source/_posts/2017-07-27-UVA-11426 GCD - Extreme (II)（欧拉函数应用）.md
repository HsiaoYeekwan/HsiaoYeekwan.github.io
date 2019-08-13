---
layout: post
title: UVA-11426 GCD - Extreme (II)（欧拉函数应用）
category: ACM
tags: [ACM,欧拉函数]
updated: 2017-07-27
date: 2017-07-27
---

## 题目传送门 ： [UVA-11426](https://vjudge.net/problem/UVA-11426)

## 题意：
>给定n，求解G<br/>
```c
G=0;
for(i=1;i<N;i++)
  for(j=i+1;j<=N;j++)
  {
    G+=gcd(i,j);
  }
```
<!--more-->

## 思路：
>已知前n-1个数的解ans[n-1], 则ans[n] = ans[n-1] + gcd(1,n)+gcd(2,n)+···+gcd(n-1,n)<br/>
设p[n]为n的欧拉函数值，如果gcd(x,n) == 1 , 则所有gcd(x,n)== 1 的和为 p[n]<br/>
同理如果gcd(x,n) == y <=> gcd(x/y,n/y) == 1 , 即和为y*p[n/y] <br/>
令f[n] = gcd(1,n)+gcd(2,n)+···+gcd(n-1,n) <br/>
f[n] = sum(i * p[n/i])，其中i能被n整除。   <br/>
既然i能被n整除，可以用筛法求出所有的f[n] , 复杂度为nlogn.<br/>

## 欧拉函数：
>欧拉函数p[n]表示小于n且与n互质的数的数目。<br/>
直接求解：<br/>
```c
int solve(int n){
    int m = (int)sqrt(n+0.5);
    int ans = n;
    for(int i = 2 ; i <= m ; i ++){
        if(n % i == 0){
            ans = ans / i * (i - 1);
            while(n % i == 0){
                n /= i;
            }
        }
    }
    if(n > 1)ans = ans / n * (n-1);
    return ans;
}
```
nloglogn复杂度预处理：<br/>
```c
void init() {
    memset(p, 0, sizeof(p));
    p[1] = 1;
    for (int i = 2 ; i < maxn ; i ++ ) {
        if (!p[i]) {
            for (int j = i ; j < maxn ; j += i) {
                if (!p[j])p[j] = j;
                p[j] = p[j] / i * (i - 1);
            }
        }
    }
}
```
## Code：
```c
#include <iostream>
#include <cstring>
#include <cmath>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <vector>
#include <cctype>
#include <set>
#include <ctype.h>
#include <string>
#define inf 1000000000

using namespace std;
typedef long long ll;
typedef pair<int, int> P;
const int maxn = 4000000 + 10;
int p[maxn];
ll ans[maxn];
ll f[maxn];
void init() {
    memset(p, 0, sizeof(p));
    p[1] = 1;
    for (int i = 2 ; i < maxn ; i ++ ) {
        if (!p[i]) {
            for (int j = i ; j < maxn ; j += i) {
                if (!p[j])p[j] = j;
                p[j] = p[j] / i * (i - 1);
            }
        }
    }
    for(int i = 1 ; i < maxn ; i ++){
        for(int j = i+i ; j < maxn ; j += i ){
            f[j] += i * (p[j/i]);
        }
    }
    for(int i = 1 ; i < maxn ; i ++){
        ans[i] = ans[i-1] + f[i];
    }
}
int main(int argc, char const *argv[])
{
    ios::sync_with_stdio(false);
    init();
    int n;
    while(cin >> n && n){
        cout << ans[n] << endl;
    }
    return 0;
}

```
