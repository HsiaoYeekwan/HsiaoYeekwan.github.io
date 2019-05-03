---
title: SDKD 2019 Spring Training Series C3 11th Round
date: 2019-05-03 11:00:00
tags: []
category: ACM
mathjax: true
---

## Problems
>Codeforces 1151E 难度：难 类型：思维+数学
>Codeforces 1133E 难度：中等 类型：dp
>Codeforces 1153D 难度：中等 类型：dp
>Codeforces 1153B 难度：简单 类型：模拟
>Codeforces 1154C 难度：简单 类型：思路
>Codeforces 1154E 难度：中等 类型： 数据结构
<!--more-->

### A codeforces 1151E
>题意：给定一条链，去掉f(l,r)表示去掉权值不在[l,r]范围内的点，有多少个联通块求$$\sum_{l=1}^n\sum_{r=l}^n{f(l,r)}$$
>思路：对于每个点，考虑当他作为联通块第一个点的时候的贡献，分情况讨论即可

```c
#include <iostream> 
using namespace std;
typedef long long ll;
const int maxn = 2e5 + 10;
ll a[maxn];
int n;
int main(){
    cin >> n;
    for(int i = 1 ; i <= n ; i ++){
        scanf("%I64d" , &a[i]);
    }
    long long ans = a[1] * (n - a[1]+1);
    for(int i = 2 ; i <= n ; i ++){
        if(a[i] == a[i-1])continue;
        if(a[i] > a[i-1]){
            ans += (a[i] - a[i-1]) * (n - a[i] + 1);
        }
        else{
            ans += (a[i-1] - a[i]) * a[i];
        }
    }
    cout << ans << endl;
}
```
### B codeforces 1133E
>题意：将n个学生按照能力值分组，每组内的学生能力值差距不能超过5，问最多分k组时最多能包含多少学生。
>思路：动态规划。先把学生按照能力值排序，dp[i][j]表示前i个学生至多分j组时包含多少学生，最后答案即为dp[n][k]，状态转移很简单，类似背包。

```c
#include <bits/stdc++.h>
#include <iostream>
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 5e3 + 10;
int n , k ;
int v[maxn], R[maxn];
int dp[maxn][maxn];
int main(int argc, char const *argv[])
{
    cin >> n >> k;
    for(int i = 1 ; i <= n ; i ++){
        cin >> v[i];
    }
    sort(v+1 , v+1+n);
    for(int i = 1 ; i <= n ; i ++){
        R[i] = upper_bound(v+1 , v+1+n , v[i] + 5) - (v+1);
    }
    // dp[i+R[i]-1][k] = dp[i][k-1] + (R[i] - i + 1);
    for(int i = 0; i <= n-1 ; i ++){
        for(int j = 1 ; j <= k ; j ++){
            int pos = R[i+1];
            dp[i+1][j] = max(dp[i+1][j] , dp[i][j]);
            dp[pos][j] = max(dp[pos][j] , dp[i][j-1] + R[i+1] - i);
        }
    }
    cout << dp[n][k] << endl;
    return 0;
}
```
### C codeforces 1153D
>题意：给定一个树，每个节点有max和min操作，节点的权值分别取孩子节点的最大值和最小值。这棵树孩子节点数量为k，让你给每个孩子节点分配一个1-k的数，不重复，使得跟节点的值最大。
>思路：树形dp。叶子节点最终只有一个叶子节点的值成为最后的答案，其他的值都会在过程中被max或min操作忽略掉。所以用dp[i]表示必然忽略掉几个值，最后的答案是k-dp[1]+1。状态转移当此节点是min操作，则dp[u] += dp[v],否则dp[u] = max(dp[v])。注意必然忽略掉的值必须是先忽略大的值，因为min操作是忽略掉大的值。

```c
#include <math.h>
#include <iostream>
#include <stdio.h>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 3e5 + 10;
ll n , m;

vector<int> G[maxn];
int op[maxn] , fa[maxn];
vector<int> vec;
int k;
int leaf[maxn];
int dp[maxn] , sz[maxn];
void dfs(int u , int f)
{
	dp[u] = inf;
	int cnt = 0;
	sz[u] = 0;
	for(int i = 0 ; i < G[u].size() ; i ++){
		int v = G[u][i];
		if(v != f){
			dfs(v , u);
			cnt ++;
			if(leaf[v] == 1)sz[u] ++;
		}
	}
	if(cnt == 0){
		k ++;
		leaf[u] = 1;
	}
}
void dfs2(int u , int f)
{
	
	if(leaf[u]){
		dp[u] = 1;
		return;
	}
	
	bool flag = false;
	int tot = 0;
	for(int i = 0 ; i < G[u].size() ; i ++){
		int v = G[u][i];
		if(v != f){
			dfs2(v , u);
			
			if(op[u] == 1){
				dp[u] = min(dp[v] , dp[u]);
			}
			else{
				tot += dp[v];
			}
		}
	}
	if(!op[u])dp[u] = tot;
}
int main(int argc, char const *argv[])
{
	ios::sync_with_stdio(false);
	cin >> n;
	k = 0;
	for(int i = 1 ; i <= n ; i ++){
		cin >> op[i];
	}
	for(int i = 2;  i <= n ; i ++){
		cin >> fa[i];
		G[fa[i]].push_back(i);
	}
	dfs(1 , -1);
	dfs2(1 , -1);
	cout << k - dp[1] + 1 << endl;
	return 0;
}
```

### D codeforces 1153B
>思路：按照题意模拟即可，输出任意解即可

```c
#include <math.h>
#include <iostream>
#include <stdio.h>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 100 + 10;
ll n , m;
struct node
{
	int x,t;
};
bool cmp(const node &a , const node &b){
	return a.x < b.x;
}
int a[maxn] , b[maxn];
int maps[maxn][maxn];
int main(int argc, char const *argv[])
{
	ios::sync_with_stdio(false);
	ll h;
	cin >> n >> m >> h;
	for(int i =1 ; i <= m ; i ++){
		cin >> b[i];
	}
	for(int i = 1; i <= n ; i ++){
		cin >> a[i];
	}
	for(int i = 1 ; i <= n ; i ++){
		for(int j = 1 ; j <= m ; j ++){
			cin >> maps[i][j];
		}
	}
	for(int i = 1 ; i <= n ; i ++){
		for(int j = 1 ; j <= m ; j ++){
			if(maps[i][j] == 1){
				int Min = min(b[j] , a[i]);
				maps[i][j] = Min;
			}
		}
	}
	for(int i = 1 ; i <= n ; i ++){
		for(int j = 1 ; j <= m ; j ++){
			printf("%d%c" , maps[i][j] , j == m ? '\n' : ' ');
		}
	}
	return 0;
}
```
### E codeforces 1154C
>思路：先取模，然后暴力即可，思考一下复杂度不高

```c
#include <math.h>
#include <iostream>
#include <stdio.h>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 1e5 + 10;
ll n , m;
int cnt[] = {1 , 2, 3 , 1 , 3 , 2, 1};
ll solve(ll a , ll b , ll c , ll k)
{
	int num = 0;
	for(int i = k ; ; i ++){
		if(i >= 7)i %= 7;
		if(cnt[i] == 1){
			if(a == 0)break;
			a --;
		}
		if(cnt[i] == 2){
			if(!b)break;
			b --;
		}
		if(cnt[i] == 3){
			if(!c)break;
			c --;
		}
		num ++;
		
	}
	return num;
}
int main(int argc, char const *argv[])
{
	ll a,b,c;
	cin >> a>> b >> c;
	ll q = min(a/3 , min(b/2 , c/2));
	ll ans = q * 7;
	a -= q * 3 , b -= q * 2 , c -= q * 2;
	ll p = 0;
	for(int i = 0 ; i < 7 ; i ++){
		p = max(p , solve(a,b,c,i));
	}
	cout << ans + p << endl;
	return 0;
}
```

### E codeforces 1154E
>思路：对于连续区间的覆盖可以用线段树，因为后一次覆盖的区间一定包含上一次覆盖的区间，所以最后反转更新，每个点暴力查询即可。中间查询覆盖左右端点可以二分。
```c
#include <math.h>
#include <iostream>
#include <stdio.h>
#include <bits/stdc++.h>
#define inf 0x3f3f3f3f
using namespace std;
typedef long long ll;
typedef pair<int,int> P;
const int maxn = 2e5 + 10;
ll n , m , k;
int lazy[maxn << 2];
int sum[maxn << 2] , a[maxn] , p[maxn];
void push_down(int l , int r , int o)
{
    if(lazy[o]){
        int mid = (l + r) >> 1;
        sum[o*2] = (mid - l + 1)*lazy[o];
        sum[o*2+1] = (r - mid)*lazy[o];
        lazy[o*2] = lazy[o];
        lazy[o*2+1] = lazy[o];
        lazy[o] = 0;
    }
    return ;
}
void build(int o , int l , int r)
{
    lazy[o] = 0;
    if(l == r){
        sum[o] = 0;
        return;
    }
    int mid = (l + r) >> 1;
    build(o*2 , l , mid);
    build(o*2+1 , mid + 1 , r);
    sum[o] = sum[o*2] + sum[o*2+1];
}
ll query(int o , int l , int r , int ql , int qr)
{
    if(ql <= l && r <= qr){
        return sum[o];
    }
    int mid = (l + r) >> 1;
    push_down(l , r,  o);
    ll ans1 = 0;
    ll ans2 = 0;
    if(ql <= mid)ans1 = query(o*2 , l , mid , ql , qr);
    if(qr > mid)ans2 = query(o*2 + 1 , mid +1 , r , ql , qr);
    return ans1 + ans2;
}
void update(int o , int l , int r , int ql , int qr , ll c)
{
    if(ql <= l && r <= qr){
        sum[o] = (r - l + 1) * c;
        lazy[o] = c;
        return;
    }
    int mid = (l + r) >> 1;
    push_down(l , r, o);
    if(ql <= mid){
        update(o << 1 , l , mid , ql , qr , c);
    }
    if(qr > mid){
        update(o << 1 | 1 , mid + 1 , r , ql , qr , c);
    }
    sum[o] = sum[o*2] + sum[o*2+1];
    return ;
}
int getsum(int l , int r)
{
	if(l > r)return 0;
	int tot = r - l + 1;
	return tot - query(1 , 1 , n , l , r);
}
int FindL(int pos){
	int L = 1;
	if(getsum(1 , pos-1) <= k)return 1;
	int R = pos - k;
	int ans = pos - 1;
	while(L <= R){
		int mid = L + R >> 1;
		if(getsum(mid , pos-1) <= k){
			ans = mid;
			R = mid - 1;
		}
		else{
			L = mid + 1;
		}
	}
	return ans;
}
int FindR(int pos){
	int R = n;
	if(getsum(pos + 1 , R) <= k)return n;
	int L = pos + 1;
	int ans = pos + 1;
	while(L <= R){
		int mid = L + R >> 1;
		if(getsum(pos+1 , mid) <= k){
			ans = mid;
			L = mid + 1;
		}
		else{
			R = mid - 1;
		}
	}
	return ans;
}
struct node
{
	int L , R , op;
};
vector<node> c;
int main(int argc, char const *argv[])
{
	cin >> n >> k;
	for(int i = 1 ; i <= n ; i ++){
		scanf("%d" , &a[i]);
		p[a[i]] = i;
	}
	build(1 , 1 , n);
	int op = 1;
	for(int i = n ; i >= 1 ; i --){
		if(query(1 , 1, n , p[i] , p[i]) != 0)continue;
		if(op == 1){
			int L = FindL(p[i]);
			int R = FindR(p[i]);
			update(1 , 1,  n , L , R , 1);
			c.push_back(node{L , R , op});
		}
		else{
			int L = FindL(p[i]);
			int R = FindR(p[i]);
			update(1 , 1 , n , L , R , 1);
			c.push_back(node{L , R , op});
		}
		op = 3 - op;
	}
	reverse(c.begin() , c.end());
	build(1 , 1 ,n);
	for(int i = 0 ; i < c.size() ; i ++){
		update(1 , 1 , n , c[i].L , c[i].R , c[i].op);
	}
	for(int i = 1 ; i <= n ; i ++){
		printf("%d" , query(1 , 1 , n , i , i));
	}
	cout << endl;
	return 0;
}
```