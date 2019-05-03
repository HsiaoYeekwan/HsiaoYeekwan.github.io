---
title: HDU1069-Monkey and Banana-(简单dp)
date: 2019-03-08 11:00:00
tags: [sdust_training_2019_03_08]
category: ACM
mathjax: true
---

## 题目链接
[hdu-1069](https://cn.vjudge.net/problem/HDU-1069)
<!--more-->


## 思路
>每个长方体有6种不同的摆法，然后就是一个LIS问题


```c
#include <iostream>
#include <cstring>
#include <cmath>
#include <cstdio>
#include <string>
#include <algorithm>

using namespace std;
typedef long long ll;
const int maxn = 30*6+10;
struct node{
	int x,y,z;
};
node a[maxn];
node t[maxn];
int dp[maxn];
bool cmp(const node &a,const node &b){
	return (a.x > b.x || (a.x == b.x && a.y > b.y));
}
int n;
int main(int argc, char const *argv[])
{
	int kase = 0;
	while(cin >> n && n){
		int cnt = 0;
		for(int i = 0 ; i < n ; i ++){
			int x,y,z;
			cin >> x >> y >> z;
			a[cnt].x = x , a[cnt].y = y , a[cnt++].z = z;
			a[cnt].x = x , a[cnt].y = z , a[cnt++].z = y;
			a[cnt].x = z , a[cnt].y = y , a[cnt++].z = x;
			a[cnt].x = y , a[cnt].y = z , a[cnt++].z = x;
			a[cnt].x = z , a[cnt].y = x , a[cnt++].z = y;
			a[cnt].x = y , a[cnt].y = x , a[cnt++].z = z;
		}
		sort(a,a+cnt,cmp);
		for(int i = 0 ; i < cnt ; i ++){
			dp[i] = a[i].z;
		}
		for(int i = 1 ; i < cnt ; i ++){
			for(int j = 0 ; j < i ; j++){
				if(a[j].x > a[i].x && a[j].y > a[i].y && dp[i] - a[i].z < dp[j]){
					dp[i] = dp[j]+a[i].z;
				}
			}
		}
		int Max = 0;
		for(int i = 0 ; i < cnt ; i ++){
			Max = max(Max,dp[i]);
		}
		printf("Case %d: maximum height = %d\n",++kase,Max );
	}
	return 0;
}
```
