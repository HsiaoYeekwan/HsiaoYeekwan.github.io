---
title: ZOJ-3201-Tree of Tree
date: 2019-03-08 11:00:00
tags: [sdust_training_2019_03_08]
category: ACM
mathjax: true
---

## 题目链接
[ZOJ-3201](https://cn.vjudge.net/problem/ZOJ-3201)
<!--more-->

## 题意
>有一个n个点的树，每个点有个权值，问大小为k的子树的最大权值和为多少。

## 思路
>设dp[x][j]为x点大小为j的子树的最大权值和,则dp[x][j] = max(dp[x][j], dp[x][k] + dp[v][j - k]);
>第一层倒序循环的原因是避免v及其子树重复计算，类似于01背包一维的思想


```c
#include <iostream>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 100 + 10;
int n , m;

struct node
{
	int to;
	double val;
	node(){}
	node(int tt,double vv):to(tt),val(vv){}
};
double d[maxn];
int vis[maxn] , cnt[maxn];
vector<int> G[maxn];
int dp[maxn][maxn];
int v[maxn];
void dfs(int u , int fa)
{
	dp[u][1] = v[u];
	for(int i = 0 ; i < G[u].size() ; i ++){
		int v = G[u][i];
		if(v == fa)continue;
		dfs(v , u);
		for(int j = m ; j >= 1 ; j --){
			for(int k = 1 ; k < j ; k ++){
				dp[u][j] = max(dp[u][j] , dp[u][k] + dp[v][j-k]);
			}
		}
	}
}
int main(int argc, char const *argv[])
{
	while(cin >> n >> m){
		for(int i = 0 ; i < n ; i ++)cin >> v[i];
		for(int i = 0 ; i < maxn ; i ++)G[i].clear();
		memset(dp , 0 , sizeof(dp));
		for(int i = 0 ; i < n-1 ; i ++){
			int u , v;
			cin >> u >> v;
			G[u].push_back(v);
			G[v].push_back(u);
		}
		dfs(0,-1);
		int Max = 0;
		for(int i = 0; i < n ; i ++){
			Max = max(Max , dp[i][m]);
		}
		cout << Max << endl;
	}
	return 0;
}
```
