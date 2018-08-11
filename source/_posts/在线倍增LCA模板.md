---
title: 在线倍增LCA模板
date: 2018-08-09 21:20:56
tags: [LCA,模板]
category: ACM
mathjax: true
---
<!--more-->

```c
void dfs(int u, int fa)
{
    for(int i = 0 ; i < G[u].size() ; i ++)
    {
        int v =G[u][i];
        if(v == fa) continue;
        if(! dep[v])
            dep[v] = dep[u] + 1, p[v][0] = u, dfs(v, u);
    }
}
void lca_init()
{
    memset(dep, 0, sizeof dep);
    int root;//一定要选择入度为0的点为根节点
    for(int i = 1; i <= n; i++)
        if(in[i] == 0) root = i;
    dep[root] = 1;
    dfs(root, -1);
    for(int j = 1; (1<<j) <= n; j++)
        for(int i = 1; i <= n; i++)
            p[i][j] = p[p[i][j-1]][j-1];
}
int LCA(int v, int u)
{
    if(dep[v] < dep[u]) swap(v, u);
    int d = dep[v] - dep[u];
    for(int i = 0; (d>>i) != 0; i++)
        if((d>>i) & 1) v = p[v][i];
    if(v == u) return v;
    for(int i = 20; i >= 0; i--)
        if(p[v][i] != p[u][i]) v = p[v][i], u = p[u][i];
    return p[v][0];
}
```
