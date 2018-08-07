---
layout: post
title: UVA-1153 Keep the Customer Satisfied(贪心)
category: ACM
tags: [ACM,贪心]
updated: 2018-01-30 
---

## 题目传送门 ：
 [UVA-1153](https://vjudge.net/problem/UVA-1153)

 ### 思路:
>因为所求的最多串行执行任务数，很容易想到先执行结束时间越早的任务;
 所以先按照结束时间对任务进行排序。
 从第一个任务开始枚举,我们可以记录一下完成当前任务完成后的时间，如果超出
 了结束时间，也就是说我们必须舍弃一个任务，根据贪心策略，舍弃的任务的执行时间
 尽量长，以便于给后面的任务空出尽量多的时间。（因为我们求的只是最多可串行任务数量）.
 所以可以用一个优先队列存一下执行过任务的执行时间，舍弃任务的时候便从队头元素取。
 队列中的元素数量即是答案。

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
#define inf 0x3f3f3f3f
#define eps 0.000001
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 1e6+10;
const int mod = 1e9+7;
int n;
priority_queue<int , vector<int> ,less<int> > pq;
struct node
{
    int t,e;
    node(){}
    node(int tt, int ee ):t(tt),e(ee){}
    bool operator < (const node &a) const{
        return e < a.e;
    }
};
node a[maxn];
bool cmp(const node &a,const node &b){return a.e < b.e;}
int main()
{
    int T;
    cin >> T;
    while(T--){
        scanf("%d",&n);
        for(int i = 0 ; i < n ; i ++){
            scanf("%d%d",&a[i].t , &a[i].e);
        }
        sort(a , a + n , cmp);
        while(!pq.empty())pq.pop();
        int ans = 0;
        int now = 0;
        for(int i = 0 ; i < n ; i ++){
            now += a[i].t;
            pq.push(a[i].t);
            if(now > a[i].e){
                now -= pq.top();
                pq.pop();
            }
            else{
                ans ++;
            }
        }
        cout << ans << endl;
        if(T)cout << endl;
    }
    return 0;
}
 ```
