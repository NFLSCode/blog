---
title: 'Polyquine —— 同时在多种语言运行的自生成程式'
categories: [notes]
tags: [Cpp,Python]
toc_level: 5
date: 2022-07-19 21:17:33
---

以下源代码遵循 CC BY-NC-ND 4.0 开源协议。

Quine AC记录（初版）：

[C++14](https://hydro.ac/d/zcoj/record/62cd7d02e4874952a0851613) [Python3](https://hydro.ac/d/zcoj/record/62cd7d18e4874952a0851624)

以后打算写一下原理。各版本原理不同，都打算写一下。

感谢 @[GlaceonVGC](https://www.luogu.com.cn/user/538464) 的帮助，没有他该程序不可能实现。

<!--more-->

当前版本：

ver 8（2warning，gnu++ only）

```py
#import<cstdio>/*
'''*/
main(){auto/*'''
def printf(a,*b):print(a%b,end='')#*/
_="#import<cstdio>/*%c'''*/%cmain(){auto/*'''%cdef printf(a,*b):print(a%%b,end='')#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10);%c#/*%c{#*/%c}";printf(_,10,10,10,10,34,_,34,10,10,10);
#/*
{#*/
}
```

```cpp
#import<cstdio>/*
'''*/
main(){auto/*'''
def printf(a,*b):print(a%b,end='')#*/
_="#import<cstdio>/*%c'''*/%cmain(){auto/*'''%cdef printf(a,*b):print(a%%b,end='')#*/%c_=%c%s%c;printf(_,10,10,10,10,34,_,34,10,10,10);%c#/*%c{#*/%c}";printf(_,10,10,10,10,34,_,34,10,10,10);
#/*
{#*/
}
```