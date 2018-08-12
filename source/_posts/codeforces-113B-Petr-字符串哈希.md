---
title: 'codeforces 113B Petr#(字符串哈希)'
date: 2018-08-12 11:34:43
tags: [ACM,字符串,哈希]
category: ACM
---
## 题目链接
[codeforces 113B](http://codeforces.com/problemset/problem/113/B)

<!--more-->

### 题意
>给出三个字符串s,st,ed
问有多少个连续子串的前缀是st，后缀是ed
字符串长度均小于2000

### 思路
>因为要求的是连续子串，很容易想到hash
从st串与s串匹配的位置开始，暴力往后寻找与ed串匹配的位置，将hash值插入到vector里面最后计数即可
需要注意有可能匹配到的ed串的尾部可能会小于st串的尾部，这种情况需要特殊处理一下
直接unsigned long long自动取模就可以了
```c
#include <iostream>
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int maxn = 2000 + 10;
const ll p = 1e9+7;
char s[maxn] , st[maxn] , ed[maxn];
ull hash1[maxn] , f[maxn] , hashst[maxn] , hashed[maxn];
void init()
{
    f[0] = 1;
    for(int i = 1 ; i < maxn ; i ++){
        f[i] = f[i-1] * p;
    }
}
void Hash(char *s , ull *hash1)
{
    int len = strlen(s);

    for(int i = 1 ; i <= len ; i ++){
        hash1[i] = hash1[i-1] * p + (s[i-1] - 'a' + 1);
    }
}
ull getHash(int l , int r , ull *hash1)
{
    ull tmp = hash1[r] - hash1[l-1] * f[r-l+1];
    return tmp;
}

int main()
{
    scanf("%s%s%s" , s , st , ed);
    init();
    int len1 = strlen(st);
    int len2 = strlen(ed);
    Hash(s , hash1) ;
    Hash(st , hashst) ;
    Hash(ed , hashed);
    ll q1 = getHash(1 , len1 , hashst);
    ll q2 = getHash(1 , len2 , hashed);
    vector<ull> vec;

    int n = strlen(s);
    for(int i = 1 ; i <= n ; i ++){

        if(i + len1 - 1 > n)break;

        if(q1 == getHash(i , i + len1-1 , hash1)){

            for(int j = i ; j <= n ; j ++){
                if(j + len2 - 1 > n)break;
                if(j + len2 - 1 < i + len1 - 1)continue;//判断ed字符串的结尾是否小于st字符串的结尾
                if(q2 == getHash(j , j + len2 - 1 , hash1)){
                    vec.push_back(getHash(i , j + len2 - 1 , hash1));
                }
            }
        }
    }
    sort(vec.begin() , vec.end());
    int ans = unique(vec.begin() , vec.end()) - vec.begin();
    printf("%d\n" , ans);
    return 0;
}
```