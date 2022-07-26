---
title: '七彩树 题解'
categories: [solution]
tags: [BZOJ]
toc_level: 3
date: 2022-07-18 09:49:00
---

[题目传送门](https://hydro.ac/d/bzoj/p/4771/)

让我们先解决几个问题。

## 树链问题

当树是一条链时，问题变为区间问题。

<!--more-->

设结点的下标为其深度，对于每一个点，开一棵线段树维护这样的序列。

在父亲线段树的基础上，在当前结点下标上加一，同时在上一次出现该结点值的地方减一。

查询时在下标为 $dep_x+d$ 的那棵线段树上查询 $[x,n+1)$ 即可。

可以用主席树实现。

（SDOI2009 HH的项链）

我们发现，其实**查询 $[l,r+1)$ 就够了**。

## 子树问题

每次询问的深度都充分大时，问题变为询问一个子树的问题。

我们扩展上面的做法。观察发现一棵子树在 dfs 序上连续，从而把问题转化为 dfs 序上的区间问题。

## 原问题

这样的询问无法像上面那样转化为区间问题了。

不过可以这么考虑：如果先更新深度小的线段树再更新大的，那岂不是说在一棵树上**查询任何区间的答案都只统计到当前深度**？

所以我们按 bfs 序去刷新线段树，但是线段树内仍维护 dfs 序，查询时查询**待查集合内 dfs 序最靠后的结点就行了**？

所以思路就是，线段树上维护 dfs 序，但是每个结点从 bfs 序的前一个更新而来，每次颜色相同的、dfs 序上和它距离最小的结点减一。查询的时候在子树内深度 $\leq dep_x+d$ 且在 dfs 序上最靠后的那棵线段树上查询以 $x$ 为根的子树这段 dfs 序即可。

不好意思，错了。

```
6 1
1 2 3 3 4 5
1 2 1 4 4
4 2
```

这时候是这样一棵树：

![树的示意图](https://hydro.ac/d/bzoj/file/2579/graph.png)

每个结点上线段树的值：

```
// bfs序
1: 100000
2: 110000
4: 110100
3: 111000
5: 111010
6: 111011
```

查询的是结点 $6$ 上的 $[4,6+1)$（按照 dfs 序），答案为 $2$。

事实上，是 $3$ 号结点抹掉了 $4$ 号结点的值。

仔细想想，其实询问同时包含两个结点时，才需要减一。

而同时包含两个结点的，最深是它们的 LCA。所以在 LCA 上减一。

另外，容易发现，查询待查深度的最后一棵线段树即可。此时就算线段树有更改，也一定落在这棵子树的 dfs 序外面。这样每个深度维护一棵线段树即可。

就酱。

```cpp
#include <bits/stdc++.h>
using namespace std;
// limits
const int VL{1},VR(1e5),N(1e5),L{20};
// persistent seg
struct tnode;
vector<tnode> mem;
struct pnode
{
    int id;
    tnode& operator*(){return mem[id];}
    tnode* operator->(){return &mem[id];}
    pnode& operator=(pnode n){id=n.id;return *this;}
    pnode(int p=0):id{p}{}
};
struct tnode
{
    pnode l,r;
    int sz;
};
pnode new_tnode(){mem.emplace_back();return mem.size()-1;}
pnode Insert(pnode,int,int,int);
int query(pnode,int,int,int,int);
// tree
vector<int> e[N+5];
int dfp[N+5];
pnode tree[N+5];
// node
int cl[N+5],fa[N+5][L+5];
int dep[N+5],dfn[N+5],sz[N+5],cxx;
int LCA(int,int);
// preparation
pnode init();
// color
set<int> col[N+5];
int close(int);
int main()
{
    int T;cin>>T;
    while (T--)
    {
        int n,m;cin>>n>>m;
        for (int i{1};i<=n;++i)
            scanf("%d",cl+i);
        for (int i{1};i<=n;++i)
            e[i].clear(),col[cl[i]].clear();
        for (int i{2};i<=n;++i)
        {
            int p;scanf("%d",&p);
            e[p].push_back(i);
            fa[i][0]=p;
        }
        tree[0]=init();
        vector<int> bfn;
        for (int i{1};i<=n;++i)
            bfn.push_back(i);
        sort(bfn.begin(),bfn.end(),[](int a,int b){return dep[a]<dep[b]||dep[a]==dep[b]&&dfn[a]<dfn[b];});
        int ct{0};
        for (auto u:bfn)
        {
            tree[dep[u]]=Insert(tree[ct],dfn[u],1,n);
            ct=dep[u];
            if (col[cl[u]].size()) tree[ct]=Insert(tree[ct],dfn[close(u)],-1,n);
            col[cl[u]].insert(dfn[u]);
        }
        // cout<<"qwq"<<mem[3].r.id<<endl;
        int last{0};
        while (m--)
        {
            int x,d;scanf("%d %d",&x,&d);
            x^=last;d^=last;
            printf("%d\n",last=query(tree[dep[x]+d],dfn[x],sz[x]+1,1,n+1));
        }
    }
    return 0;
}
// binary desp
int jmp(int x,int d)
{
    for (int i{0};d;++i)
    {
        if (d&1) x=fa[x][i];
        d>>=1;
    }
    return x;
}
int LCA(int a,int b)
{
    if (dep[a]<dep[b]) swap(a,b);
    a=jmp(a,dep[a]-dep[b]);
    if (a==b) return a;
    for (int i{L};i>=0;--i)
        if (fa[a][i]!=fa[b][i])
            a=fa[a][i],b=fa[b][i];
    return fa[a][0];
}
void fz_init(int u)
{
    dfn[u]=++cxx;dfp[cxx]=u;
    dep[u]=dep[fa[u][0]]+1;
    for (int i{1};i<=L;++i)
        fa[u][i]=fa[fa[u][i-1]][i-1];
    for (auto v:e[u])
        fz_init(v);
    sz[u]=cxx;
}
pnode init()
{
    cxx=0;
    fz_init(1);
    mem.clear();mem.emplace_back();
    return 0;
}
pnode Insert(pnode p,int k,int v,int cnt)
{
    pnode q{new_tnode()};
    *q=*p;
    // printf("%d: p%d,q%d\n",k,p.id,q.id);
    q->sz+=v;
    if (cnt==1) return q;
    int cp{cnt>>1};
    if (k<=cp) q->l=Insert(p->l,k,v,cp);
    else q->r=Insert(p->r,k-cp,v,cnt-cp);
    // cout<<&(q->l)<<" "<<&(mem[q.id].l)<<endl;;
    // cout<<q->l.id<<endl;
    // printf("fin %d: q%d %d\n",k,q->l,q->r);
    return q;
}
int query(pnode p,int L,int R,int pL,int pR)
{
    if (p.id==0) return 0;
    if (L<=pL&&pR<=R) return p->sz;
    int mid{pL+pR>>1},ans{0};
    if (L<mid) ans+=query(p->l,L,R,pL,mid);
    if (R>mid) ans+=query(p->r,L,R,mid,pR);
    return ans;
}
int close(int p)
{
    auto& s{col[cl[p]]};
    auto il{s.lower_bound(dfn[p])},iu{s.upper_bound(dfn[p])};
    if (il!=s.begin())
        if (iu!=s.end())
        {
            int l{LCA(dfp[*prev(il)],p)},u{LCA(dfp[*iu],p)};
            if (dep[l]>dep[u]) return l;
            else return u;
        }
        else return LCA(dfp[*prev(il)],p);
    else return LCA(dfp[*iu],p);
}
```