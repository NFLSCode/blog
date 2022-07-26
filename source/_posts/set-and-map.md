---
title: 浅谈c++中的set与map家族
categories: [notes]
tags: [Cpp]
toc_level: 2
date: 2021-07-25 16:19:54
---

众所周知，`set` 和 `map` 是两种常用的 STL 容器。

<!--more-->

## 0. 说句闲话

### 前置芝士：

- `set` 与 `map` 的熟练使用，包括遍历

### 本文包含以下内容：

| |不排序|不去重|
|---|---|---|
|`set`|`unordered_set`|`multiset`|
|`map`|`unordered_map`|`multimap`|

并且全部为 `C++11` 标准中的内容。

## 1. 不排序

`set` 和 `map` 都是用红黑树实现的，虽然是 $\log n$，但还是比较慢的。

`unordered_set` 就会快很多，它不使用平衡树，而是用哈希实现，复杂度 $O(1)$。

由于是把相同哈希值的元素放在一起，遍历时没有顺序。**并不保证按输入顺序输出**。

```c++
#include <unordered_set> //bits 也可以
using namespace std;

...

unordered_set <int> s;
 
s.insert(9);
s.insert(2);
s.insert(1);
s.insert(9);
s.insert(8);
s.insert(7);

for (auto i : s)
	cout << i << ' ';

...
```
输出：
```
7 8 1 9 2
```
注意 `unordered_set` 仍然会进行去重。

`find()`、`count()`、`size()`等和 `set` 无区别。

`unordered_map` 同 `unordered_set`，但是使用 `operator[]` 时属于修改，后插入的会替换之前的。

```c++
#include <unordered_map> //bits 也可以
using namespace std;

...

unordered_map <int, int> m;

m.insert(make_pair(9, 8)); //其实 map <Ta, Tb> 可以当作 set <pair <Ta, Tb> >
m.insert(make_pair(9, 2));
m.insert(make_pair(1, 7));
m.insert(make_pair(2, 0));
m.insert(make_pair(0, 2));
m.insert(make_pair(1, 6));
++m[0];
m[4]=6;
m[4]=7;

for(auto i : m)
    cout << i.first << ' ' << i.second << endl;
    
...

```
输出：
```
4 7
0 3
2 0
9 8
1 7
```

## 2. 不去重

`multiset` 和 `set` 基本一致，但是它不会去重。

所以调用 `find()` 会返回第一个符合要求的（插入顺序），而 `count()` 会返回所有符合要求的。

```c++
#include <set>
using namespace std;

...

multiset <int> s;

s.insert(9);
s.insert(7);
s.insert(9);
s.insert(8);
s.insert(1);
s.insert(7);

for (auto i : s)
    cout << i << ' ';

...

```
输出：
```
1 7 7 8 9 9
```

`multimap` 同 `multiset`，且没有定义 `operator[]`。

```c++
#include <map>
using namespace std;

...

multimap <int, int> m;
m.insert({9, 1});
m.insert({2, 3});
m.insert({3, 3});
m.insert({8, 0});
m.insert({9, 1});
m.insert({3, 2});

for (auto &i : m)
    cout << i.first << ' ' << i.second << endl;
    
...
```
输出：
```
2 3
3 3
3 2
8 0
9 1
9 3
```
注意相同键值按插入顺序排列。

## 彩蛋

相信一定有人问了：有没有既不排序、也不去重的 `set` / `map` 呢？

有！

~~`vector`~~ `unordered_multiset` / `unordered_multimap` 就满足这个要求。

而且，只要人品好，哈希值不怎么冲突，什么操作都是 $O(1)$ ，吊打 `vector`（特别是插入与删除）。

这不是本文的重点，有兴趣的巨佬可以自行研究。