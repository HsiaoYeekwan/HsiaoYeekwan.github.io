---
layout: post
title: UVA-1601 The Morning after Halloween（双向BFS）
category: ACM
tags: [ACM,搜索]
updated: 2017-07-26
date: 2017-07-26
---


## 题意：
>有至多三个鬼abc，要移动到ABC的位置，其中两个鬼不能在一步之内交换，不能移动到同一个格子。每四个格子中至少有一个#<br/>
比如a在（0，0），b在（1，1），a移动到（0，1），b移动到（1，2），这样算一步。<br/>
因为状态数太多，直接bfs肯定超时。一种方法是把图重新提取出来单项bfs，另一种方法是起点和终点同时bfs，效率大大增加。<br/>
因为bfs是按层拓展，起点和终点同时拓展，当出现相同状态是就return。<br/>
可以设置两个标记数组，当起点拓展的时候如果遇到终点拓展过程中已经标记过的位置，就是搜索的解。<br/>
<!--more-->

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
const int maxn = 17;
const int dx[5] = {1, 0, -1, 0, 0};//可以原地不动
const int dy[5] = {0, 1, 0, -1, 0};
char G[maxn][maxn];
bool book1[17][17][17][17][17][17];//正向BFS标记数组
bool book2[17][17][17][17][17][17];//逆向BFS标记数组
int n, m, num;
struct node {
    int x[3], y[3];
    int step;
    node(int x1 = 0, int y1 = 0 , int x2 = 0 , int y2 = 0 , int x3 = 0 , int y3 = 0 , int ss = 0) {
        x[0] = x1 , y[0] = y1;
        x[1] = x2 , y[1] = y2;
        x[2] = x3 , y[2] = y3;
        step = ss;
    }
};
bool isin(int x1, int y1, int x2, int y2, int x3, int y3){
    if(x1 >= 0 && x1 < n && x2 >= 0 && x2 < n && x3 >= 0 && x3 < n &&
       y1 >= 0 && y1 < m && y2 >= 0 && y2 < m && y3 >= 0 && y3 < m)
        return true;
    else
        return false;
}
bool ok(const node &a , const node &b) {
    if (a.x[0] == b.x[0] && a.x[1] == b.x[1] && a.x[2] == b.x[2] && a.y[0] == b.y[0] && a.y[1] == b.y[1] && a.y[2] == b.y[2])
        return true;
    else
        return false;
}
bool vis1(int x1, int y1, int x2, int y2, int x3, int y3) {
    return book1[x1][y1][x2][y2][x3][y3];
}
bool vis2(int x1, int y1, int x2, int y2, int x3, int y3) {
    return book2[x1][y1][x2][y2][x3][y3];
}
void mark1(int x1, int y1, int x2, int y2, int x3, int y3) {
    book1[x1][y1][x2][y2][x3][y3] = true;
}
void mark2(int x1, int y1, int x2, int y2, int x3, int y3) {
    book2[x1][y1][x2][y2][x3][y3] = true;
}
queue<node> q1;
queue<node> q2;
bool bfs1() {
    int size1 = q1.size();
    while (size1--) {
        node now1 = q1.front();
        q1.pop();
        int ax = now1.x[0] , ay = now1.y[0];
        int bx = now1.x[1] , by = now1.y[1];
        int cx = now1.x[2] , cy = now1.y[2];
        for (int i = 0 ; i < 5 ; i ++) {
            int nax = ax + dx[i];
            int nay = ay + dy[i];
            if(G[nax][nay] == '#')continue;
            if(!isin(nax,nay,bx,by,cx,cy))continue;
            if (num == 1) {
                if (vis1(nax, nay, bx, by, cx, cy))continue;
                mark1(nax, nay, bx, by, cx, cy);
                if(vis2(nax, nay, bx, by, cx, cy))return true;
                q1.push(node(nax, nay, bx, by, cx, cy, now1.step + 1));
                continue;
            }
            for(int j = 0 ; j < 5 ; j ++){
                int nbx = bx + dx[j];
                int nby = by + dy[j];
                if(!isin(nax,nay,nbx,nby,cx,cy))continue;
                if(G[nbx][nby] == '#')continue;
                if(nax == bx && nay == by && ax == nbx && ay == nby)continue;
                if(nax == nbx && nay == nby)continue;
                if(num == 2){
                    if(vis1(nax,nay,nbx,nby,cx,cy))continue;
                    mark1(nax,nay,nbx,nby,cx,cy);
                    if(vis2(nax, nay, nbx, nby, cx, cy))return true;
                    q1.push(node(nax,nay,nbx,nby,cx,cy,now1.step + 1));
                    continue;
                }
                for(int k = 0 ; k < 5 ; k ++){
                    int ncx = cx + dx[k];
                    int ncy = cy + dy[k];
                    if(G[ncx][ncy] == '#')continue;
                    if(!isin(nax,nay,nbx,nby,ncx,ncy))continue;
                    if(nax == cx && nay == cy && ax == ncx && ay == ncy)continue;
                    if(ncx == bx && ncy == by && cx == nbx && cy == nby)continue;
                    if(nax == ncx && nay == ncy)continue;
                    if(nbx == ncx && nby == ncy)continue;
                    if(vis1(nax,nay,nbx,nby,ncx,ncy))continue;
                    mark1(nax,nay,nbx,nby,ncx,ncy);
                    if(vis2(nax, nay, nbx, nby, ncx, ncy))return true;
                    q1.push(node(nax,nay,nbx,nby,ncx,ncy,now1.step+1));
                }
            }
        }
    }
    return false;
}
bool bfs2() {
    int size2 = q2.size();
    while (size2--) {
        node now2 = q2.front();
        q2.pop();
        int ax = now2.x[0] , ay = now2.y[0];
        int bx = now2.x[1] , by = now2.y[1];
        int cx = now2.x[2] , cy = now2.y[2];
        for (int i = 0 ; i < 5 ; i ++) {
            int nax = ax + dx[i];
            int nay = ay + dy[i];
            if(G[nax][nay] == '#')continue;
            if (num == 1) {
                if (vis2(nax, nay, bx, by, cx, cy))continue;
                mark2(nax, nay, bx, by, cx, cy);
                if(vis1(nax, nay, bx, by, cx, cy))return true;
                q2.push(node(nax, nay, bx, by, cx, cy, now2.step + 1));
                continue;
            }
            for(int j = 0 ; j < 5 ; j ++){
                int nbx = bx + dx[j];
                int nby = by + dy[j];
                if(G[nbx][nby] == '#')continue;
                if(nax == bx && nay == by && ax == nbx && ay == nby)continue;
                if(nax == nbx && nay == nby)continue;
                if(num == 2){
                    if(vis2(nax,nay,nbx,nby,cx,cy))continue;
                    mark2(nax,nay,nbx,nby,cx,cy);
                    if(vis1(nax, nay, nbx, nby, cx, cy))return true;
                    q2.push(node(nax,nay,nbx,nby,cx,cy,now2.step + 1));
                    continue;
                }
                for(int k = 0 ; k < 5 ; k ++){
                    int ncx = cx + dx[k];
                    int ncy = cy + dy[k];
                    if(G[ncx][ncy] == '#')continue;
                    if(nax == cx && nay == cy && ax == ncx && ay == ncy)continue;
                    if(ncx == bx && ncy == by && cx == nbx && cy == nby)continue;
                    if(nax == ncx && nay == ncy)continue;
                    if(nbx == ncx && nby == ncy)continue;
                    if(vis2(nax,nay,nbx,nby,ncx,ncy))continue;
                    mark2(nax,nay,nbx,nby,ncx,ncy);
                    if(vis1(nax, nay, nbx, nby, ncx, ncy))return true;
                    q2.push(node(nax,nay,nbx,nby,ncx,ncy,now2.step+1));
                }
            }
        }
    }
    return false;
}
int bfs(const node &a,const node &b){
    int ans = 0;
    while(!q1.empty()){q1.pop();}
    while(!q2.empty()){q2.pop();}
    q1.push(a);
    q2.push(b);
    mark1(a.x[0],a.y[0],a.x[1],a.y[1],a.x[2],a.y[2]);
    mark2(b.x[0],b.y[0],b.x[1],b.y[1],b.x[2],b.y[2]);
    bool ok = false;
    while(q1.size() && q2.size()){
        if(bfs1()){ok = true ;ans++; break;}
        else ans ++;
        if(bfs2()){ok = true ; ans++;break;}
        else ans ++;
    }
    return ok ? ans : -1;
}
void init() {
    memset(book1, false, sizeof(book1));
    memset(book2, false, sizeof(book2));
}
int main(int argc, char const *argv[])
{
  //注意使用取消流同步不能使用scanf和gets，会导致WA。
    //ios::sync_with_stdio(false);
    while (cin >> m >> n >> num && n && m && num) {
        init();
        getchar();
        node start, End;
        for (int i = 0 ; i < n ; i ++) {
            gets(G[i]);
        }
        for (int i = 0 ; i < n ; i ++) {
            for (int j = 0 ; j < m ; j ++) {
                if (islower(G[i][j])) {
                    int v = G[i][j] - 'a';
                    start.x[v] = i;
                    start.y[v] = j;
                }
                else if (isupper(G[i][j])) {
                    int v = G[i][j] - 'A';
                    End.x[v] = i;
                    End.y[v] = j;
                }
            }
        }
        int ans = bfs(start,End);
        cout << ans << endl;
    }
    return 0;
}
```
