---
layout: post
title: POJ 2376 Cleaning Shifts（区间覆盖，贪心）
category: ACM
tags: [ACM,贪心]
updated: 2017-07-03
---

## 题目传送门 ：[POJ 2376](http://poj.org/problem?id=2376)
>思路：
* 按照左端点排序
* 从左到右选择尽量大的右端点连续更新
* 注意更新完毕后r是不是大于等于T，否则便是未完全覆盖
* 第一个点的左端点如果不是1，也是不符合题意
* 在连续更新的过程中，如果连续更新的maxr小于等于上一次更新到的r，
说明区间之间有空隙，也是未完全覆盖。

<!--more-->
# code：
```c
#include <iostream>
#include <cstring>
#include <cmath>
#include <cstdio>
#include <string>
#include <algorithm>
#include <queue>
#include <vector>
#include <set>
#define inf 99999999

using namespace std;
typedef long long ll;

const int maxn = 1000000+10;
struct node{
    int l,r;
};
node a[100100];
bool cmp(const node &a,const node &b){
    return (a.l < b.l)||(a.l == b.l && a.r < b.r);
}

int main(int argc, char const *argv[])
{
    int T,n;
    while(cin >> n >> T){
        for(int i = 0 ; i < n ; i ++){
            scanf("%d%d",&a[i].l,&a[i].r);
        }
        sort(a,a+n,cmp);
        if(a[0].l > 1){cout << "-1" << endl;continue;}
        int sum = 1 , now = 0;
        while(a[now].l == 1){now ++;}
        int r = a[now-1].r , maxr = a[now-1].r;
        while(now < n){
            while(now < n && a[now].l <= r + 1){
                maxr = max(maxr,a[now].r);
                now ++;
            }
            if(maxr <= r)break;//合适区间的右端点没有更新，即区间之间有空隙，不符合要求。
            sum ++;
            r = maxr;
        }
        if(r >= T){cout << sum << endl;}
        else {cout << "-1" << endl;}
    }
    return 0;
}
/*  
5 10
1 5
1 7
3 10
5 9
9 10
output
2
*/
```
