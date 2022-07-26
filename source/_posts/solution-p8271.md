---
title: '[USACO22OPEN]COW Operations S 题解'
categories: [solution]
tags: [Luogu]
toc_level: 3
date: 2022-03-28 07:37:38
---

[题目传送门](https://www.luogu.com.cn/problem/P8271)

# 性质

首先，三个字母是可以随意交换的，所以先用 `XYZ` 代替 `COW`。

<!--more-->
 
枚举易得以下性质（`=>` 意为**变换后答案不变**，`=x>` 反之，`E` 表示空串）：

1. `X =x> Y`
1. `XY =x> X/Y`
1. `XYZ =x> X/Y/Z`
1. `XX =x> X`
1. `XY => Z`（逆运算）
1. `E => XX`（逆运算）

整合一下就得到了三个字母各最多有一个的解决方案：

|$s$|$ans$|理由|
|:-:|:-:|:-:|
|`E`|`N`|性质 4、6|
|`C`|`Y`|-|
|`O`|`N`|性质 1|
|`W`|`N`|性质 1|
|`CO`|`N`|性质 2|
|`CW`|`N`|性质 2|
|`OW`|`Y`|性质 5|
|`COW`|`N`|性质 3|

于是我们的目标变为：尝试将字符串 $s$ 简化为上述情况中的一种。

---
接下来，让我们请出伟大的性质  7：

7. `XY => Z => YX`（性质 5）
7. `XX => XX`

也就是说，**任意两个相邻的的字符可以互相交换**。

这意味着什么呢？

冒泡排序学过没？

也就是说，**这个字符串可以被任意的重新排列**。

相信到这里各位都会做了。

# 解法
我们将字符串 $s$ 排序，把相邻的删掉，最后每种字符最多有一个。完了。

用前缀和处理一下，对于每个询问，统计区间内的三个字符的个数除以 2 的余数，再根据上面的表格求出答案就行了。

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXN(2e5+5);
int sum[MAXN][3];
int c['~'];
int main()
{
    c['C']=0;c['O']=1;c['W']=2;
    string s;cin>>s;
    for (int i{1};i<=s.size();++i)
    {
        for (int j{0};j<3;++j) sum[i][j]+=sum[i-1][j];
        ++sum[i][c[s[i-1]]];
    }
    int T;cin>>T;
    while (T--)
    {
        int l,r;scanf("%d %d",&l,&r);
        int z[3];
        for (int i{0};i<3;++i)
            z[i]=sum[r][i]-sum[l-1][i]&1;
        printf("%c",!(z[1]^z[2])&&(z[0]^z[1])?'Y':'N');
    }
    return 0;
}
```