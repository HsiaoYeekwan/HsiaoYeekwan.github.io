---
layout: post
title: POJ-3463（Dijkstra次短路+计算最短and次短路径数量）
category: ACM
tags: [ACM,最短路]
updated: 2017-07-20
---
# 题目传送门 ： [POJ 3463](http://poj.org/problem?id=3463)

## 题意：
>输入n，m，代表n个节点的图，下面m行每行三个数，代表从a到b的长度为c，在输入s，t，求从s到t的最短路和次短路的数目。<br/>
题目要求最短路的长度为最短路的长度+1，才为合格的次短路。<br/>
其中，用Dijkstra进行计算的时候同时保存最短路和次短路的长度，最后算出次短路的长度<br/>
最后在检测次短路长度是否合法。<br/>
cnt数组记录最短路和次短路的数量<br/>
Attention:在用Dijkstra进行最短路计数的时候，一定要标记已经访问过的节点，避免重复导致计算结果增多。
<!--more-->

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
const int maxn = 100000 + 10;
struct node {
    int to;
    int val;
    node(int tt, int vv): to(tt), val(vv) {}
};
typedef pair<int, int> P;
vector<node> G[maxn];

int d[maxn][2];
bool book[maxn][2];
int cnt[maxn][2];

int n, m, s, t ;
void dij(int s) {
    for (int i = 1 ; i <= n ; i++) {
        d[i][0] = d[i][1] = inf;
    }
    d[s][0] = 0;
    priority_queue<P , vector<P> , greater<P> > q;
    q.push(P(0, s));
    while (!q.empty()) {
        P now = q.top(); q.pop();
        int u = now.second;
        if (now.first > d[u][1])continue;
        for (int i = 0 ; i < G[u].size() ; i ++) {
            node e = G[u][i];
            int dis = e.val + now.first;
            if (d[e.to][0] > dis) {
                swap(d[e.to][0], dis);
                q.push(P(d[e.to][0], e.to));
            }
            if (d[e.to][1] > dis && d[e.to][0] < dis ) {//次短路长度必须小于最短路的长度
                d[e.to][1] = dis;
                q.push(P(d[e.to][1], e.to));
            }
        }

    }
}
void solve(int s) {
    memset(cnt,0,sizeof(cnt));
    memset(book,false,sizeof(book));
    cnt[s][0] = 1;
    priority_queue<P, vector<P> , greater<P> > q;
    q.push(P(0,s));
    while(!q.empty()){
        P now = q.top(); q.pop();
        int u = now.second;
        if(book[u][1])continue;
        int flag = book[u][0];
        book[u][flag] = 1;
        if(d[u][flag] >= inf)continue;
        for(int i = 0 ; i < G[u].size() ; i ++){
            node e = G[u][i];
            int dis = e.val + now.first;
            if(d[e.to][0] == d[u][flag] + e.val){
                q.push(P(d[e.to][0],e.to));
                cnt[e.to][0] += cnt[u][flag];
            }
            if(d[e.to][1] == d[u][flag] + e.val){
                q.push(P(d[e.to][1],e.to));
                cnt[e.to][1] += cnt[u][flag];
            }
        }
    }
}
int main(int argc, char const *argv[])
{
    ios :: sync_with_stdio(false);
    int T;
    cin >> T;
    while (T--) {
        cin >> n >> m;
        for (int i = 0 ; i < maxn ; i ++) {
            G[i].clear();
        }
        for (int i = 0 ; i < m ; i ++) {
            int x, y, z;
            cin >> x >> y >> z;
            G[x].push_back(node(y, z));
        }
        cin >> s >> t;
        dij(s);
        solve(s);
        int ans = cnt[t][0];
        if(d[t][1] == d[t][0] + 1) ans += cnt[t][1];
        cout << ans << endl;
    }
    return 0;
}
```
