---
title: hdu6397 Character Encoding(容斥)
date: 2018-08-15 21:40:57
tags: [ACM,数学,容斥]
category: ACM
mathjax: true
---

## 题目链接
[HDU-6397](https://vjudge.net/problem/HDU-6397)
<!--more-->

## 题意
><big>求解$\sum_{i=1}^m x_i=k$
><big>其中 $0 \leq x_i \leq {n-1} $ 的解得个数,然后取模
><big>根据隔板法，所有答案为
$$
\begin{pmatrix}
	{k+m-1}\\\\
	{m-1}\\\\
\end{pmatrix}
$$
>假设至少有i个位置值域不符合要求的个数：
$$
\begin{pmatrix}
	{m}\\\\
	{i}\\\\
\end{pmatrix}
\begin{pmatrix}
	{k+m-1-i*n}\\\\
	{m-1}\\\\
\end{pmatrix}
$$

```c
#include <iostream>
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;
const int maxn = 2e5+10;
const int mod = 998244353;
ll n , m , k;
ll fact[maxn] , inv[maxn] , f[maxn];
void init()
{
    fact[0] = f[0] = inv[1] = inv[0] = 1;
    for(int i = 2 ; i < maxn ; i ++){

        inv[i] = inv[mod%i] * (mod - mod/i)%mod;
    }
    for(int i = 1; i < maxn ; i ++){
        fact[i] = (fact[i-1] * i) % mod;
        f[i] = (f[i-1] * inv[i])%mod;
    }
}
ll C(int x , int y)
{
    if(x < y)return 0;
    return fact[x]*(f[y]*f[x-y]%mod)%mod;
}
int main()
{
    int T;
    cin >> T;
    init();
    while(T--){
        cin >> n >> m >> k;
        ll ans = C(m+k-1 , m-1);
        for(int i = 1 ; i <= m ; i ++){
            if(k - i*n < 0)break;
            ll tmp;
            if(i & 1){
                tmp = C(m,i) * C(m+k-1-i*n , m-1) % mod;
                ans = (((ans - tmp) % mod) + mod)%mod;
            }
            else{
                tmp = C(m,i) * C(m+k-1-i*n , m-1) % mod;
                ans = (((ans + tmp) % mod) + mod)%mod;
            }
        }
        printf("%lld\n" , ans);
    }
    return 0;
}

```
