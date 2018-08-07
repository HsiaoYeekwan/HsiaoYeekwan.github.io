---
layout: post
title: HDU-4081 Qin Shi Huang's National Road System（Kruskal+次小生成树）
category: ACM
tags: [ACM,最小生成树]
updated: 2017-07-26
---

## 题目传送门 ：[HDU-4081](http://acm.hdu.edu.cn/showproblem.php?pid=4081)

## 题意 ：
>有n个城市，秦始皇要修用n-1条路把它们连起来，要求从任一点出发，都可以到达其它的任意点。<br/>
秦始皇希望这所有n-1条路长度之和最短。然后徐福突然有冒出来，说是他有魔法，可以不用人力、财力就变出其中任意一条路出来。<br/>
秦始皇希望徐福能把要修的n-1条路中最长的那条变出来，但是徐福希望能把连接人口数量最多的那条变出来。<br/>
对于每条路连接的人口数，是指这条路连接的两个城市的人数之和。<br/>
最终，秦始皇给出了一个公式，A/B，A是指要徐福用魔法变出的那条路所需人力，<br/>
B是指除了徐福变出来的那条之外的所有n-2条路径长度之和，选使得A/B值最大的那条。
<!--more-->
## 思路：
>很明显的次小生成树。<br/>
先求最小生成树，然后枚举任意两个点之间修建魔法道路，<br/>
同时去掉两个点之间的在最小生成树之间的距离最大的道路。<br/>
先用bfs处理d数组，d[i][j]表示从点i到点j的在生成树上的路径中的最大边<br/>
## 反思
>今天的组队赛打的很烂，最后这个题也没想出来，想歪了。<br/>
想到了最小生成树，可是怎么想也是n^3的复杂度<br/>
原来可以用BFS预处理一下已经求出来的最小生成树上的两个点之间的最长的边<br/>
知识面不够广，思维需要拓展，希望在暑假集训后自己的思维能有提升。

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
const int maxn = 1000+10;
const int dx[5] = {1, 0, -1, 0, 0};
const int dy[5] = {0, 1, 0, -1, 0};
struct node{
    int x,y;
    int id;
    int num;
    node(int xx = 0  , int yy = 0 , int nn = 0 ):x(xx),y(yy),num(nn){}
};
struct node2{
    int from,to;
    double d;
    node2(int ff = 0 , int tt = 0 , double dd = 0){
        from = ff;
        to = tt;
        d = dd;
    }
};
node a[maxn];
int f[maxn];
int h[maxn];
double d[maxn][maxn];
bool book[maxn];
int find(int x){
    return f[x] == x ? x : f[x] = find(f[x]);
}
void unite(int x, int y){
    x = find(x);
    y = find(y);
    if(x == y)return;
    if(h[x] < h[y]){
        f[x] = y;
    }
    else{
        f[y] = x;
        if(h[x] == h[y])
            h[x] ++;
    }
}
bool same(int x,int y){
    return find(x) == find(y);
}
double rood(int x1,int y1,int x2 , int y2){
    return sqrt((double)(x1-x2)*(x1-x2) + (double)(y1-y2)*(y1-y2));
}
bool cmp(const node2 &a , const node2 &b){
    return a.d < b.d;
}
int n;
vector<node2> G;
vector<int> GG[maxn];
void bfs(int now){
    queue<int> q;
    memset(book,false,sizeof(book));
    q.push(now);
    book[now] = true;
    while(!q.empty()){
        int p = q.front();
        q.pop();
        for(int i = 0 ; i < GG[p].size() ; i++){
            int End = GG[p][i];
            if(book[End])continue;
            book[End] = true;
            d[now][End] = max(d[now][p] , rood(a[p].x,a[p].y,a[End].x,a[End].y));
            q.push(End);
        }
    }
}

void init(){
    G.clear();
    for(int i = 0 ; i < maxn ; i ++){
        f[i] = i;
        h[i] = 0;
        GG[i].clear();
    }
    memset(d,0,sizeof(d));
}
int main(int argc, char const *argv[])
{
    int T;
    ios::sync_with_stdio(false);
    cin >> T;
    while(T--){
        init();
        cin >> n;
        for(int i = 0 ; i < n ; i ++){
            cin >> a[i].x >> a[i].y >> a[i].num;
            a[i].id = i;
        }
        for(int i = 0 ; i < n ; i ++){
            for(int j = i + 1 ; j < n ; j ++){
                G.push_back(node2(a[i].id,a[j].id,rood(a[i].x,a[i].y,a[j].x,a[j].y)));
            }
        }
        double distance = 0;
        sort(G.begin() , G.end() , cmp);
        for(int i = 0 ; i < G.size() ; i ++){
            node2 now = G[i];
            if(!same(now.from , now.to)){
                unite(now.from,now.to);
                GG[now.from].push_back(now.to);
                GG[now.to].push_back(now.from);
                distance += now.d;
            }
        }
        for(int i = 0 ; i < n ; i ++){
            bfs(i);
        }
        double ans = 0;
        for(int i = 0 ; i < n ; i ++){
            for(int j = i+1 ; j < n ; j ++){
                ans = max( (a[i].num+a[j].num)/(distance - d[i][j]),ans);
            }
        }
        printf("%.2f\n", ans);
    }
    return 0;
}
```
