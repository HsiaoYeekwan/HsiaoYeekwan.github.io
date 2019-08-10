---
layout: post
title: HDU-3342 Legal or Not(拓扑排序判断环)
category: ACM
tags: [ACM,拓扑排序,模板]
updated: 2017-07-10
---
# 题目传送门 ： [HDU-3342](http://acm.hdu.edu.cn/showproblem.php?pid=3342)


>题意：
输入n,m。n代表所需要判断的人数，m是关系的数量，输入a,b代表a是b的师傅，判断输入是否合法，也就是说输入
1,2、2,3、3,1是不合法的，裸的拓扑排序，存在环就是输出NO，否则YES。
<!--more-->

### 拓扑排序判断环的方法（DFS）：
```c
bool dfs(int u){
    book[u] = -1;//此处代表u这个节点正在递归进程中。
    for(int i = 0 ; i < n ; i ++){
        if(G[u][i]){
            if(book[i] == -1)return false;//如果此点已经走过，代表存在环。
            else if(book[i] == 0){
                if(!dfs(i))return false;//对于没有访问到的点递归访问
            }
        }
    }
    book[u] = 1;//访问完成标记。
    topo[--t] = u;//topo数组保存拓扑排序的顺序。
    return true;
}
bool toopsort(){
    memset(book,0,sizeof(book));
    t = n;
    for(int i = 0 ; i < n ; i ++){
        if(!book[i]){
            if(!dfs(i))return false;
        }
    }
    return true;
}
```
### HDU-3342 code：
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
const int maxn = 100+10;
bool G[maxn][maxn];
int book[maxn];
int n,m;

bool dfs(int u){
    book[u] = -1;
    for(int i = 0 ; i < n ; i ++){
        if(G[u][i]){
            if(book[i] == -1)return false;
            else if(book[i] == 0){
                if(!dfs(i))return false;
            }
        }
    }
    book[u] = 1;
    return true;
}
bool toopsort(){
    memset(book,0,sizeof(book));
    for(int i = 0 ; i < n ; i ++){
        if(!book[i]){
            if(!dfs(i))return false;
        }
    }
    return true;
}
int main(int argc, char const *argv[])
{
    while(cin >> n >> m && n && m){
        memset(G,false,sizeof(G));
        for(int i = 0 ; i < m ; i++){
            int x,y;
            cin >> x >> y;
            G[x][y] = true;
        }
        if(toopsort())cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```
