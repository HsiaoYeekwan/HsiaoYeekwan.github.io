---
layout: post
title: UVA-11149 Power of Matrix（矩阵运算-倍增法）
category: ACM
tags: [ACM,倍增]
updated: 2017-07-27
---

## 题目传送门 ： [UVA-11149](https://vjudge.net/problem/UVA-11149)

## 题意 ：
>已知矩阵n行n列的矩阵A ， 求:<br/>
![](https://img-blog.csdn.net/20160308092952312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)<br/>
因为只输出矩阵元素的最后一位数字，所以对10取模。<br/>
<!--more-->

## 思路：
>按照二分法的思想，原式 =<br/>
![](https://img-blog.csdn.net/20160308094241171?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)<br/>
提取公因式
https://img-blog.csdn.net/20160308094353923?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center
而A^1+A^2+······+A^(n/2)可以继续按照上式化简，总的复杂度为logn<br/>

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
const int maxn = 40+10;
int n,k;
struct mat{
    int c[maxn][maxn];
    mat(){
        for(int i = 0 ; i < maxn ; i ++){
            for(int j = 0 ; j < maxn ; j ++){
                c[i][j] = 0;
            }
        }
    }
    friend mat operator + (mat a,mat b){
        mat t;
        for(int i = 0 ; i < n ; i ++){
            for(int j = 0 ; j < n ; j ++){
                t.c[i][j] = (a.c[i][j] + b.c[i][j])%10;
            }
        }
        return t;
    }
};
mat start;
mat mul(mat a,mat b){
    mat ans;
    for(int i = 0 ; i < n ; i ++){
        for(int j = 0 ; j < n ; j ++){
            for(int k = 0 ; k < n ; k ++){
                ans.c[i][j] = (ans.c[i][j]+a.c[i][k] * b.c[k][j])%10;
            }
        }
    }
    return ans;
}
mat Pow(mat a, int p){
    mat ans;
    for(int i = 0 ; i < n ; i ++){
        ans.c[i][i] = 1;
    }
    while(p){
        if(p & 1){
            ans = mul(ans,a);
        }
        a = mul(a,a);
        p >>= 1;
    }
    return ans;
}
//倍增法求解
mat solve(mat a,int p){
    if(p == 1)return a;
    mat t,sum;
    t = solve(a,p/2);
    sum = t + mul(Pow(a,p/2) , t);
    if(p & 1){
        sum = sum + Pow(a,p);//如果p是奇数要在加上A^p
    }
    return sum;
}
void shuchu(mat a){
    for(int i = 0  ; i < n ; i ++){
        for(int j = 0 ; j < n ; j ++){
            printf("%d%c" , a.c[i][j] , j == n-1 ? '\n' : ' ');
        }
    }
}
int main(int argc, char const *argv[])
{
    ios::sync_with_stdio(false);
    while(cin >> n >> k && (n)){
        for(int i = 0 ; i < n ; i ++){
            for(int j = 0 ; j < n ; j ++){
                cin >> start.c[i][j];
                start.c[i][j] %=10;
            }
        }
        mat ans = solve(start,k);
        shuchu(ans);
        printf("\n");
    }
    return 0;
}
```
