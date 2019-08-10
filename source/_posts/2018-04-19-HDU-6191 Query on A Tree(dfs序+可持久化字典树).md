---
layout: post
title: HDU-6191 Query on A Tree(dfs序+可持久化字典树)
category: ACM
tags: [ACM,字典树]
---
# 题目传送门 ： [HDU 6191](https://cn.vjudge.net/problem/HDU-6191)

## 题意
>给出一棵有向树，根为1，q个查询，每次输入u,x；代表查询u和u的子树中与x异或的最大值，输出这个最大值。<br/>
首先考虑dfs序,u及u的子树可以用一个区间[L,R]代替，L表示u的时间戳，R表示u的字树中节点数量。<br/>
然后建立一棵可持久化字典树查询即可<br/>
root[i]表示插入到第i个数的时候trie的节点编号。

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
#define inf 1e17
using namespace std;
typedef long long ll;
typedef pair<ll,ll> P;
const int maxn = 100000 + 10;
int n , m;
vector<int> G[maxn];
struct node
{
    int ch[maxn * 32][2];
    int sum[maxn*32];
    int cnt;
    void init()
    {
        cnt = 0;
        memset(ch , 0 , sizeof(ch));
        memset(sum , 0 , sizeof(sum));
    }
    int Insert(int x,int val)
    {
        int temp,y;
        temp = y = ++cnt;
        for(int i = 30 ; i >= 0 ; i --){
            ch[y][0] = ch[x][0];
            ch[y][1] = ch[x][1];
            sum[y] = sum[x] + 1;
            int t = (val & (1 << i));
            t >>= i;
            x = ch[x][t];
            ch[y][t] = ++cnt;
            y = ch[y][t];
        }
        sum[y] = sum[x] + 1;
        return temp;
    }
    int Query(int l , int r , int val)
    {
        int res = 0;
        for(int i = 30 ; i >= 0 ; i --){
            int t = ( val & (1 << i) );
            t >>= i;
            if(sum[ch[r][t^1]] - sum[ch[l][t^1]]){
                l = ch[l][t^1];
                r = ch[r][t^1];
                res += (1 << i);
            }
            else{
                l = ch[l][t];
                r = ch[r][t];
            }
        }
        return res;
    }
};
node trie;
int v[maxn] , d[maxn] , have[maxn] , val[maxn] , root[maxn] ;
int dfn[maxn];
int index;
void dfs(int u)
{
    dfn[u] = ++index;
    have[u] = 1;
    for(int i = 0 ; i < G[u].size() ; i ++){
        int v = G[u][i];
        dfs(v);
        have[u] += have[v];
    }
}
void init()
{
    index = 0;
    for(int i = 0 ; i < maxn ; i ++){
        G[i].clear();
    }
    trie.init();
    memset(have , 0 , sizeof(have));
}
int main()
{
    while(cin >> n >> m){
        init();
        for(int i = 1 ; i <= n ; i ++){
            scanf("%d",&v[i]);
        }
        for(int i = 1 ; i <= n-1 ; i ++){
            int x;
            scanf("%d",&x);
            G[x].push_back(i+1);
        }
        dfs(1);
        for(int i = 1 ; i <= n ; i ++){
            val[dfn[i]] = v[i];
        }
        for(int i = 1 ; i <= n ; i ++){
            root[i] = trie.Insert(root[i-1] , val[i]);
        }
        while(m --){
            int k , x;
            scanf("%d%d" , &k , &x);
            int l = dfn[k];
            int r = dfn[k] + have[k] - 1;
            printf("%d\n" , trie.Query(root[l-1] , root[r] , x));
        }
    }
    return 0;
}
```
