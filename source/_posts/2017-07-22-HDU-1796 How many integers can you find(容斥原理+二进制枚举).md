---
layout: post
title: HDU-1796 How many integers can you find(容斥原理+二进制枚举)
category: ACM
tags: [ACM,容斥]
updated: 2017-07-22
---

# 题目传送门 ： [HDU-1796](http://acm.hdu.edu.cn/showproblem.php?pid=1796)


## 题意：
>输入n，m；然后输入m个数的集合。<br/>
求小于n，且可以被m个数中任意一个整除的数有哪些？<br/>
因为会有重复，很容易想到容斥原理。小于n的每一个数能够被几个m中的数整除。奇加偶减。<br/>
求出这几个数的最小公倍数lcm,符合题意的数有(n-1)/lcm个。<br/>
<!--more-->

## Code:
```c
#include <iostream>
#include <cstring>
#include <cmath>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <vector>
#include <time.h>
#include <set>
#define inf 10000000

using namespace std;
typedef long long ll;
const int maxn = 10+10;
ll a[maxn];
ll n,m;
ll gcd(ll a, ll b){
    return b == 0 ? a : gcd(b, a % b);
}
ll lcm(ll a , ll b){
    return a * b / gcd(a,b);
}
int main(int argc, char const *argv[])
{
    while(cin >> n >> m){
        int cnt = 0;
        for(int i = 0 ; i < m ; i ++){
            ll x;
            cin >> x;//注意输入会有0的情况。
            if(x > 0){a[cnt++] = x;}
        }
        m = cnt;
        ll ans = 0;
        for(int i = 1  ; i < (1 << m) ; i ++){
            ll k = 0;
            ll temp = 1;
            for(int j = 0 ; j < m ; j ++){
                if(i & (1 << j)){
                    k ++;
                    temp = lcm(temp , a[j]);
                }
            }
            if(k % 2 == 0)
                ans -= (n-1)/temp;
            else
                ans += (n-1)/temp;
        }
        cout << ans << endl;
    }
    return 0;
}
```
