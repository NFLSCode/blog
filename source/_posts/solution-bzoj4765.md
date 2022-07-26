---
title: '普通计算姬 题解'
categories: [solution]
tags: [BZOJ]
toc_level: 3
date: 2022-07-24 15:27:00
---

[题目传送门](https://hydro.ac/d/bzoj/p/4765/)

序列分块。对于每一个块预处理出其答案。

考虑修改时对每一个块的贡献。因为每个点只对它祖先的答案有贡献，所以我们可以遍历整棵树，开一个桶记录当前每一个块的数个数。遍历到 x 时，将该桶的内容就是 x 对每个块的贡献，记录下来即可。这样修改时统计每个块 sum 加了多少就是贡献乘值的变化，$O(\sqrt{n})+\text{node modify}$。

查询时整块加起来，散块暴力求出每个点的子树和，$O(\sqrt{n})(1+\text{tree query})$。

显然我们需要一个数据结构维护 dfs 序上的单点加区间求和。

考虑根号平衡，分块维护前缀和数组，这样 $\text{node modify}$ 就相当于后缀加，$O(\sqrt{n})$，$\text{tree query}$ 就相当于单点查询，$\Theta(1)$。

如此，修改、查询复杂度为 $O(\sqrt{n})$，总复杂度 $O(n\sqrt{n})$。

显然不同块对答案的贡献可以独立计算，所以可以离线下来，每个块单独处理。这样空间 $\Theta(n\sqrt{n})\to\Theta(n)$（虽然 $\Theta(n\sqrt{n})$）也能通过）。

~~我这么懒的人肯定只会写在线~~

```cpp
#include <bits/stdc++.h>
using namespace std;
// #define int uLL
using uLL=unsigned long long;
inline int read() {
    char c;int x;
    do x=(c=getchar())^48;
    while (!isdigit(c));
    while (isdigit(c=getchar()))
    x=(x<<3)+(x<<1)+(c^48);
    return x;
}
const int N(1e5),B{316},K{N/B+1};
int a[N+5];
int n,gf[N+5][B+5],dfn[N+5],fz[N+5],dR[N+5];
uLL val[N+5],tag[B+5],res[B+5];
vector<int> e[N+5];
inline int bl(int x){return x?(x-1)/K+1:0;}
inline int R(int b){return min(n,b*K);}
inline int L(int b){return R(b-1)+1;}
uLL query(int l,int r) {
    --l;
    return val[r]+tag[bl(r)]-val[l]-tag[bl(l)];
}
void modify(int p,int v) {
    for (int i{p};i<=R(bl(p));++i) val[i]+=v;
    for (int i{bl(p)+1};i<=bl(n);++i) tag[i]+=v;
}
uLL ppip(int l,int r) {
    uLL ans{0};
    for (int i{l};i<=r;++i) ans+=query(dfn[i],dR[i]-1);
    return ans;
}
void fz_init(int u,int fa) {
    static int cxx{1},bf[B+5]{};
    fz[cxx]=u;dfn[u]=cxx++;
    ++bf[bl(u)];
    copy_n(bf+1,bl(n),gf[u]+1);
    for (auto v:e[u])
        if (v!=fa)
            fz_init(v,u);
    --bf[bl(u)];
    dR[u]=cxx;
}
int main() {
    // freopen("4765/1.in","r",stdin);
    int n{read()},m{read()};::n=n;
    for (int i{1};i<=n;++i)
        a[i]=read();

    for (int i{1};i<=n;++i) {
        int u{read()},v{read()};
        e[u].push_back(v);
        e[v].push_back(u);
    }
    fz_init(e[0][0],0);
    for (int i{1};i<=n;++i)
        val[i]=val[i-1]+a[fz[i]];
    for (int i{1};i<=n;++i)
        res[bl(i)]+=query(dfn[i],dR[i]-1);
    // for (int i{1};i<=bl(n);++i) cout<<gf[2000][i]<<endl;
    while (m--) {
        int op{read()},x{read()},y{read()};
        if (op==1) {
            a[x]+=y-=a[x];
            modify(dfn[x],y);
            for (int i{1};i<=bl(n);++i)
                res[i]+=(uLL)y*gf[x][i];
        } else {
            uLL ans;
            if (bl(x)==bl(y)) {
                ans=ppip(x,y);
            } else {
                ans=ppip(x,R(bl(x)))+ppip(L(bl(y)),y);
                for (int i{bl(x)+1};i<bl(y);++i)
                    ans+=res[i];
            }
            printf("%llu\n",ans);
        }
    }
    return 0;
}
```