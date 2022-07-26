---
title: '[POI2008]POC-Trains 题解'
categories: [solution]
tags: [Luogu]
toc_level: 3
date: 2022-05-24 20:20:15
---

[题目传送门](https://www.luogu.com.cn/problem/P3471)

这是一个 $O(nl+m)$ 的做法（即和输入同阶）。

<!--more-->

首先，看到字符串相同显然想到哈希。我们开一个哈希表，每个键值维护一个 `size`。对于每次修改，从表中原位置删除，重新计算哈希然后插入到新位置。

更新的话，只要在删除的时候查一下在当前的键值下，从插入到现在这段时间中 `size` 的最大值。

所以，我们只需要在线支持如下操作：

- 查询后缀最大值
- 在结尾插入一个数

显然可以开一个并查集维护（如果不会带权并查集可以看[这个](https://www.luogu.com.cn/blog/374433/union-find)）。

```cpp
// 返回的不是祖先而是 `x` 这个点的后缀最大值（其实所有点的祖先都是最后插入的数）
int Find(int u,int x)
{
    if (dsu[u][x].fa==x) return dsu[u][x].mx;
    dsu[u][x].mx=max(dsu[u][x].mx,Find(u,dsu[u][x].fa));
    dsu[u][x].fa=dsu[u][dsu[u][x].fa].fa;
    return dsu[u][x].mx;
}
void insert(int x)
{
    int H{h[x]};// h[x] 为串 x 的哈希
    // 动态开 vector，主要是开表大小个 vector 内存会炸
    if (!F[H]) F[H]=cnt++;
    // 插入的位置（即删除时要查询的位置）
    lst[x]=dsu[F[H]].size();
    ++sz[H];
    // 将前面的树 merge 到新插入的点
    if (lst[x]) dsu[F[H]][lst[x]-1].fa=lst[x];
    // 插入新节点
    dsu[F[H]].emplace_back(lst[x],sz[H]);
}
```

因为只有一棵树且没有 `merge`，所以这份并查集的复杂度是线性的。

最后还要再更新一下最大值。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int bn{2579},b6e0{25165843};
const int N(1e3),M(1e5);
int sz[b6e0],res[N];
int lst[N];
int F[b6e0];
struct node
{
	int fa,mx;
	node(int _fa=0,int _mx=0):fa{_fa},mx{_mx}{}
};
vector<node> dsu[N+M];int cnt;
int h[N],pl[N],l;
string s[N];
int Find(int u,int x)
{
	if (dsu[u][x].fa==x) return dsu[u][x].mx;
	dsu[u][x].mx=max(dsu[u][x].mx,Find(u,dsu[u][x].fa));
	dsu[u][x].fa=dsu[u][dsu[u][x].fa].fa;
	return dsu[u][x].mx;
}
void fresh(int x,int p,int n)
{
    h[x]=(h[x]+1LL*pl[l-p-1]*(n-s[x][p])%b6e0)%b6e0;
    if (h[x]<0) h[x]=(h[x]+b6e0)%b6e0;
    s[x][p]=n;
}
void upd(int x)
{
	int H{h[x]};
	if (!F[H]) F[H]=cnt++;
	lst[x]=dsu[F[H]].size();
	++sz[H];
    if (lst[x]) dsu[F[H]][lst[x]-1].fa=lst[x];
	dsu[F[H]].emplace_back(lst[x],sz[H]);
	
}
int main()
{
    int n,m;cin>>n>>l>>m;
    for (int i{0};i<n;++i)
    {
        cin>>s[i];
        for (char c:s[i])
            h[i]=(1LL*h[i]*bn%b6e0+c-'a')%b6e0;
        upd(i); 
    }
    pl[0]=1;
    for (int i{1};i<=l;++i)
        pl[i]=1LL*pl[i-1]*bn%b6e0;
    while (m--)
    {
        int a,i,b,j;
        scanf("%d %d %d %d",&a,&i,&b,&j);
        --a;--i;--b;--j;
        --sz[h[a]];res[a]=max(res[a],Find(F[h[a]],lst[a]));
        if (a!=b) --sz[h[b]],res[b]=max(res[b],Find(F[h[b]],lst[b]));
        char x{s[a][i]},y{s[b][j]};
        fresh(a,i,y);fresh(b,j,x);
        upd(a);if (a!=b) upd(b);
    }
    for (int i{0};i<n;++i)
        cout<<max(res[i],Find(F[h[i]],lst[i]))<<endl;
    return 0;
}
```