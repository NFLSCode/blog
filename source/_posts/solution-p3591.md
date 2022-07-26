---
title: '[POI2015]ODW 题解'
categories: [solution]
tags: [Luogu]
toc_level: 3
date: 2022-07-18 09:45:32
---

[题目传送门](https://www.luogu.com.cn/problem/P3591)

这是一个树剖+序列分块。感觉因为单纯树剖跑不满 log，比树上k级祖先+根号分治的做法要快一些。

<!--more-->

对于查询的两个端点，记余数 $m_u,m_v$ 为 $1$。

每次树剖跳 $u$ 的时候，在序列上查询这一段模数为 $k$，且下标（从1开始）余数为 $m_u$ 的和，再令 $m_u=(m_u-l)\mod k$（$l$ 是查询区间的长度）。跳 $v$ 同理。

对 dfs 序进行分块，块大小为 $S$。每块对于 $1\leq k\leq S$ 的模数预处理出下标（从1开始）余数为 $0\leq m<k$ 的和。

散块暴力跳，跑完每个块，余数类似树剖那段进行变化。

如果查询的 $k\geq S$，就直接暴力跳整个区间。

一个小问题是树剖是倒着跳的，在正的 dfs 序上查询余数有点麻烦。

所以我们把每个点在 dfs 序上的位置变为 `n-dfn[x]+1`，相当于分块维护反 dfs 序。

本质类似根号分治做法，但充分利用了树剖的特性而不是每次就只跳 $S$。

取 $S=\sqrt{n}$，时间 $O(n\lg n\sqrt{n})$，空间 $\Theta(n\sqrt{n})$。

```cpp
#include <bits/stdc++.h>
using namespace std;
inline int read(){
    char c;int x,f{0};
    do x=(c=getchar())^48;
    while (!isdigit(c)&&c!='-');
    if (x==29) f=-1,x=0;
    while (isdigit(c=getchar()))
        x=(x<<3)+(x<<1)+(c^48);
    return (x^f)-f;
}
const int N(5e4),B{223*20},K{N/B+1};
// val是dfs序上的点权
int sum[B+5][K][K],val[N+5],a[N+5],n;
inline int R(int b){return b*K;}
inline int L(int b){return R(b-1)+1;}
inline int bl(int x){return (x-1)/K+1;};
int ppip(int l,int r,int k,int m)
{
    if (!m) m=k;
    int ans{0};
    for (int i{l+m-1};i<=r;i+=k)
        ans+=val[i];
    return ans;
}
// 序列分块
int query(int l,int r,int k,int m){
    if (bl(l)==bl(r)) return ppip(l,r,k,m);
    int ans{ppip(l,R(bl(l)),k,m)};
    m=(m+k-(R(bl(l))-l+1)%k)%k;
    assert(R(bl(r)-1)<=n);
    for (int i{bl(l)+1};i<bl(r);++i) {
        if (k>=K) {
            if (k==K&&m==0) ans+=val[R(i)];
            else if (m<=K&&m) ans+=val[L(i)+m-1];
        } else ans+=sum[i][k][m];
        m=(m+k-K%k)%k;
    }
    ans+=ppip(L(bl(r)),r,k,m);
    return ans;
}
int fa[N+5],dep[N+5],sz[N+5],son[N+5],tp[N+5];
int dfn[N+5];
vector<int> e[N+5];
// 树剖
void fz_init(int u,int f){
    fa[u]=f;dep[u]=dep[f]+1;sz[u]=1;
    for (auto v:e[u])
        if (v!=f) {
            fz_init(v,u);
            sz[u]+=sz[v];
            if (sz[v]>sz[son[u]])
                son[u]=v;
        }
}
void fz_cut(int u,int top){
    static int cxx{1};
    tp[u]=top;dfn[u]=cxx++;
    if (son[u]) fz_cut(son[u],top);
    for (auto v:e[u])
        if (v!=fa[u]&&v!=son[u])
            fz_cut(v,v);
}
int query(int u,int v,int k){
    int wu{1%k},wv{1%k},ans{0};
    while (tp[u]!=tp[v]){
        if (dep[tp[u]]<dep[tp[v]])
            swap(u,v),swap(wu,wv);
        // 注意反dfs序反着询问
        ans+=query(dfn[u],dfn[tp[u]],k,wu);
        wu=(wu+k-(dfn[tp[u]]-dfn[u]+1)%k)%k;
        u=fa[tp[u]];
    }
    if (dep[u]<dep[v]) swap(u,v),swap(wu,wv);
    ans+=query(dfn[u],dfn[v],k,wu);
    return ans;
}
int main(){
    n=read();
    for (int i{1};i<=n;++i)
        scanf("%d",a+i);
    for (int i{1};i<n;++i) {
        int a{read()},b{read()};
        e[a].push_back(b);
        e[b].push_back(a);
    }
    fz_init(1,0);fz_cut(1,1);
    for (int i{1};i<=n;++i)
        dfn[i]=n-dfn[i]+1;
    for (int i{1};i<=n;++i)
        val[dfn[i]]=a[i];
    for (int k{1};k<K;++k)
        for (int i{2};i<bl(n);++i)
            for (int j{L(i)};j<=R(i);++j)
                sum[i][k][(j-L(i)+1)%k]+=val[j];
    for (int i{1};i<=n;++i)
        a[i]=read();
    for (int i{1};i<n;++i) {
        int w{read()};
        printf("%d\n",query(a[i],a[i+1],w));
    }
    return 0;
}
```

随便跑跑就最优解第一页了。