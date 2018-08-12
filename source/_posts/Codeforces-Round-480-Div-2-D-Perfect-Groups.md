---
title: 'Codeforces Round #480 (Div. 2) D - Perfect Groups(数学+思维)'
date: 2018-08-12 18:23:41
tags: [数学,思维,codeforces]
category: ACM
mathjax: true
---

### 题目链接
[codeforces 980D](http://codeforces.com/contest/980/problem/D)

><p>SaMer has written the greatest test case of all time for one of his problems. For a given array of integers, the problem asks to find the minimum number of groups the array can be divided into, such that the product of any pair of integers in the same group is a perfect square. </p><p>Each integer must be in exactly one group. However, integers in a group do not necessarily have to be contiguous in the array.</p><p>SaMer wishes to create more cases from the test case he already has. His test case has an array $A$ of $n$ integers, and he needs to find the number of contiguous subarrays of $A$ that have an answer to the problem equal to $k$ for each integer $k$ between $1$ and $n$ (inclusive).
<!--more-->

<big>Input
><p>The first line of input contains a single integer $n$ ($1 \leq n \leq 5000$), the size of the array.</p><p>The second line contains $n$ integers $a_1$,$a_2$,$\dots$,$a_n$ ($-10^8 \leq a_i \leq 10^8$), the values of the array.</p>

<big>Output
><p>Output $n$ space-separated integers, the $k$-th integer should be the number of contiguous subarrays of $A$ that have an answer to the problem equal to $k$.</p>

<big>Examples
Input
>2
5 5

Output
>3 0

Input
>5
5 -4 2 1 8

Output
>5 5 3 2 0

Input
>1
0

Output
>1

### 思路
>题意是给定一个序列a，对于k(1<=k<=n),输出有多少个连续子区间的值为k。子区间的值定义为把区间中的数分组，最少分几组能使得每组两两相乘的结果都是完全平方数
如果一个数的因子中有完全平方数,除掉他并不会对答案有影响。然后统计有多少个不同的数就行了，因为只有相同的数相乘才会是完全平方数。
注意0的情况需要特殊判断一下。


```c
#include <iostream>
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int maxn = 5000 + 10;
int a[maxn] , vis[maxn] , n , ans[maxn] , ct[maxn];
set<int> st;
vector<int> v;
void init()
{
    int up = 1e8;
    for(int i = 0 ; i * i<= up  ; i ++){
        st.insert(i*i);
    }
}
bool judge(int k)
{
    if(k < 0 && st.count(-k))return true;
    if(k >= 0 && st.count(k))return true;
    return false;
}
void change(int &x)
{
    int flag = 0;
    if(x < 0)flag = 1 , x = -x;
    for(int i = 2 ;i * i <= x ; i ++){
        while(x % (i*i) == 0){
            x /= (i * i);
            if(x == 1)break;
        }
    }
    if(flag)x = -x;
}
int id(int x)
{
    return lower_bound(v.begin() , v.end() , x) - v.begin();
}
int main()
{

    cin >> n;
    init();
    for(int i = 1 ;i <= n ; i ++){
        scanf("%d" , &a[i]);
    }
    for(int i = 1 ; i <= n ; i ++){
        change(a[i]);
        v.push_back(a[i]);
        if(judge(a[i]))vis[i] = 1;
    }
    sort(v.begin() , v.end());
    v.erase(unique(v.begin() , v.end()) , v.end());
    for(int i = 1;  i <= n ; i ++){
        a[i] = id(a[i]);
    }
    for(int i = 1 ; i <= n ; i ++){
        memset(ct , 0 , sizeof(ct));
        int sum = 0;
        for(int j = i; j <= n ; j ++){
            if(!ct[a[j]] && v[a[j]])sum ++ , ct[a[j]] = 1;
            ans[max(1 , sum)] ++;
        }
    }
    for(int i = 1 ; i <= n ; i ++){
        printf("%d%c" , ans[i] , i == 'n' ? '\n' : ' ');
    }
    return 0;
}

```