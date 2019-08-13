---
layout: post
title: UVA-1374 Power Calculus(IDA)
category: ACM
tags: [ACM,搜索]
updated: 2017-10-20
date: 2017-10-20
---

## 题目传送门：
[UVA-1374](httpsvjudge.netproblemUVA-1374)

## 题意
>给定一个数n，让你求从1至少要做多少次乘除才可以从 x 得到 x^n;</br>
迭代加深搜索，由已经得到的数两两加减（大于0且无重复），找到答案即break；</br>
<!--more-->

```c
#pragma comment(linker, "/STACK:102400000,102400000")
#include <iostream>
#include <cstring>
#include <cmath>
#include <cstdlib>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <map>
#include <vector>
#include <stack>
#include <cctype>
#include <set>
#include <ctype.h>
#include <string.h>
#define inf 999999999
#define eps 0.000001
using namespace std;
typedef long long ll;
const int maxn = 1000 + 10;
const int dx[] = {1, 0, -1, 0};
const int dy[] = {0, 1, 0, -1};
bool book[maxn * maxn];
int maxd, n;
vector<int> G;
int Pow(int t) {
    int qq = 1;
    for (int i = 1 ; i <= t ; i ++) {
        qq *= 2;
    }
    return qq;
}

bool dfs(int now, int d) {
    if (d > maxd)return false;
    if (now == n)return true;
    if (now * Pow(maxd - d) < n)return false;
    for (int i = 0 ; i < G.size() ; i ++) {
        int s = G[i] + now;
        int t = now - G[i];
        if (s > 0 && !book[s]) {
            book[s] = true;
            G.push_back(s);
            if (dfs(s , d + 1))return true;
            G.pop_back();
            book[s] = false;
        }
        if (t > 0 && !book[t]) {
            book[t] = true;
            G.push_back(t);
            if (dfs(t , d + 1))return true;
            G.pop_back();
            book[t] = false;
        }

    }
    return false;
}
int main(int argc, char const *argv[])
{
    while (cin >> n && n) {
        for (maxd = 0 ; ; maxd ++) {
            memset(book, false, sizeof(book));
            book[1] = true;
            G.clear();
            G.push_back(1);
            if (dfs(1, 0))break;
        }
        cout << maxd << endl;
    }
    return 0;
}

```
