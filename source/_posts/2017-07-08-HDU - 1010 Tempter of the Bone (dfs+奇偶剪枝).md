---
layout: post
title: HDU - 1010 Tempter of the Bone (dfs+奇偶剪枝)
category: ACM
tags: [ACM]
updated: 2017-07-08
---
## 题目传送门 ： [HDU-1010](http://acm.hdu.edu.cn/showproblem.php?pid=1010)

>题意：
X为障碍物，S为起点，D为终点。问可不可以__恰好__在t步数到达终点。
可以就输出YES,否则NO。

### 奇偶剪枝：
>从起点到终点的最短距离s = abs(x1-x2)+(y1-y2)，从起点到终点一定经过s + t步，其中t一定为偶数.
具体证明详见:<http://baike.baidu.com/link?url=hdOpozJVumFRtp78FhIzZVoo5E3y670MB7ujVuCnn4wBd1eWrQt4OBC65zmEvbuwE-AHZ5NIH0u9PI6da0hTLjSxwaNNgX4nlsGEqKWq8pZXYou6VWFtYljX5V9-fj7A#3>

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
#define inf 99999999

using namespace std;
typedef long long ll;
const int maxn = 10;
char G[maxn][maxn];
const int dx[4] = {1,0,-1,0};
const int dy[4] = {0,1,0,-1};
bool book[maxn][maxn];
int n,m,T;
bool ok;
void dfs(int x1,int y1,int x2,int y2,int t){
    if(x1 == x2 && y1 == y2 && t == T){ok = true;return;}
    if(t > T)return;
    int nx,ny;
    for(int i = 0 ; i < 4 ; i ++){
        nx = x1 + dx[i];
        ny = y1 + dy[i];
        if(nx < 0 || nx >= n || ny < 0 || ny >= m||book[nx][ny] || G[nx][ny] == 'X')continue;
        book[nx][ny] = true;
        dfs(nx,ny,x2,y2,t+1);
        book[nx][ny] = false;
        if(ok)return;//这里不加也会导致TLE
    }
}
int main()
{
    while(cin >> n >> m >> T && n && m && T){
        memset(book,false,sizeof(book));
        int x1,x2,y1,y2;
        for(int i = 0 ; i < n ; i ++){
            for(int j = 0 ; j < m ; j ++){
                cin >> G[i][j];
                if(G[i][j] == 'S'){
                    x1 = i;
                    y1 = j;
                }
                else if(G[i][j] == 'D'){
                    x2 = i;
                    y2 = j;
                }
            }
        }
        if(abs(x1-x2) + abs(y1-y2) > T || (T - abs(x1-x2) - abs(y1-y2)) % 2 != 0){
            cout << "NO" << endl;
            continue;
        }
        book[x1][y1] = true;
        ok = false;
        dfs(x1,y1,x2,y2,0);
        if(ok){cout << "YES"<< endl;}
        else cout << "NO" << endl;
    }
    return 0;
}
```
