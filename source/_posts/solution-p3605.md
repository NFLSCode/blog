---
title: '[USACO17JAN]Promotion Counting P 题解'
categories: [solution]
tags: [Luogu]
toc_level: 3
date: 2022-05-28 14:15:09
---

[题目传送门](https://www.luogu.com.cn/problem/P3605)

线段树合并+动态开点（指针党福利）

<!--more-->

在值域上开线段树为啥要离散化啊，我不能理解

所以这是一份无需离散化的题解。

```cpp
#include <bits/stdc++.h>
using namespace std;
struct tnode
{
    tnode *l,*r;
    // 该区间数的个数
    int cnt;
};
using pnode=tnode*;
// 哨兵节点
pnode nil{new tnode{nil,nil,0}};
void check(pnode& p){if(p==nil)p=new tnode{nil,nil,0};}
// insert操作。cnt为当前区间size，使用了类似第k大的写法
void modify(pnode& p,int k,int cnt)
{
    check(p);++p->cnt;
    if (cnt==1) return;
    int cq{cnt>>1};
    if (k<=cq) modify(p->l,k,cq);
    else modify(p->r,k-cq,cnt-cq);
}
void merge(pnode& p,pnode q)
{
    if (q==nil) return;
    if (p==nil)
    {
        p=q;
        return;
    }
    p->cnt+=q->cnt;
    merge(p->l,q->l);merge(p->r,q->r);
    // 垃圾回收
    delete q;
}
// 查询大于k的数的个数
int query(pnode p,int k,int L,int R)
{
    if (p==nil||L>k) return p->cnt;
    int mid{L+R>>1};
    // 显然如果k在右儿子里就只用查询右儿子，否则只用查询左儿子
    // 同样类似第k大
    if (k>=mid-1) return query(p->r,k,mid,R);
    else return p->r->cnt+query(p->l,k,L,mid);
}
const int N(1e5);
int v[N+5],r[N+5];
vector<int> e[N+5];
pnode fz(int u)
{
    pnode p{nil};
    for (auto v:e[u])
        merge(p,fz(v));
    r[u]=query(p,v[u],1,1e9+1);
    modify(p,v[u],1e9);
    return p;
}
int main()
{
    int n;cin>>n;
    for (int i{1};i<=n;++i)
        scanf("%d",v+i);
    for (int i{2};i<=n;++i)
    {
        int fa;scanf("%d",&fa);
        e[fa].push_back(i);
    }
    fz(1);
    for (int i{1};i<=n;++i) printf("%d\n",r[i]);
}
```