---
title: C++ 迭代器，了解一下？（2）
categories: [notes]
tags: [Cpp]
toc_level: 2
date: 2021-08-08 17:44:15
---

本文将[C++ 迭代器，了解一下？](https://www.luogu.com.cn/blog/poi/cpp-iterator) 中一笔带过的一些东西讲了一下。

本文介绍三种特殊的迭代器。除了反向迭代器，都需要包含头文件 `iterator` 。而反向迭代器需要包含相关容器的头文件。

本文的代码均使用 `C++11` 标准。

<!--more-->

## 0. 前置芝士

- 标准库函数

- 模板及相关操作

- 普通迭代器的使用

## 1. 反向迭代器

反向迭代器就是在容器中从尾元素向首元素反向移动的迭代器，它也是每个容器自己定义的。

对于所有支持反向迭代器的容器，我们都可以通过调用 `rbegin()` 以及 `rend()` 来获取反向迭代器。`rbegin()` 返回一个指向尾元素的迭代器，而 `rend()` ·返回一个首元素前一个元素的迭代器。

下图为在 `vector` 中，四个迭代器位置的关系。

![图片炸了，请私信](https://cdn.luogu.com.cn/upload/image_hosting/1qwxfmva.png)

对于反向迭代器，递增操作的含义会倒过来，如 `++it` 会使 `it` 移动到前一个元素。

那么为什么如此定义递增呢？因为这样实现的话，我们在操作迭代器时，就无需知道迭代器是正向还是反向。

```C++
// 此代码为一个反向迭代器的例子。

// 输出：
// 1 2 3 4 5 6 7 8 9
// 9 8 7 6 5 4 3 2 1


#include <iostream>
#include <vector>
using namespace std;

// 一个打印函数
template <typename T>
void print(T beg, T end)
{
    for (T it = beg; it != end; ++it)
        cout << *it << ' ';
    cout << endl;
}
int main()
{
    vector<int> v = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    print(v.begin(), v.end());
    print(v.rbegin(), v.rend());
    return 0;
}
```

如上述代码所示，我们不需要为反向迭代器单独编写一份打印函数。这正是泛型编程的特点之一。

### 反向迭代器和普通迭代器的关系

如果我们需要将一个反向迭代器转化为正向的，应该如何操作？

可以使用成员函数 `base()`。

例如，我们需要在一个 `vector` 中打印最后一个 `0` 之后的数字。

```c++
// 一个神奇的示例

// 输出：3 7 2 1

#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main()
{
    vector<int> v = {3, 5, 4, 0, 2, 6, 0, 3, 7, 2, 1};

    auto last0 = find(v.rbegin(), v.rend(), 0);

    for (auto it = last0.base(); it != v.end(); ++it)
        cout << *it << ' ';

    cout << endl;
    return 0;
}
```

这个程序有一个问题：在转换迭代器的时候，并未跳过 `0`，所以 `it` 被初始化的时候，应该第一个输出 `0`。可是它没有。

这说明：**反向迭代器在转换后有移位**。

为什么会有这种情况呢？

![图片炸了，请私信](https://cdn.luogu.com.cn/upload/image_hosting/1qwxfmva.png)
（没错还是这张图）

假设反向迭代器转换后不会移位的话：
- 如果转换 `rend()`，它对应什么？

- 转换哪一个反向迭代器，能使它变成 `end()`？

所以我们得出结论：为了保证**一一对应**，在转换后，`rbegin()` 变成 `end()`，`rend()` 变成 `begin()`。

这就是移位的由来。

还有一个问题：那普通迭代器如何转为反向迭代器？

答：反向迭代器有一个构造函数，传入一个普通迭代器。

如：
```c++
vector<int> v = {2, 1, 3};
auto it = v.begin();
vector<int>::reverse_iterator rit(it); //rit 指向 rend()
```
（不过这样比 `auto` 累多了

## 2. `iostream` 迭代器

这是极为特殊的一种迭代器。`iostream` 迭代器（或流迭代器）可以绑定流以进行输入输出。流迭代器可以通用化、简化输入输出。

### `istream_iterator`

当创建一个输入流迭代器的时候，可以绑定一个流，如 `cin`。但是不一定。未绑定的迭代器可以当作 `EOF` 使用，也可以成为 `IO` 错误的判定。 

下面是一个例子：
```c++
vector<int> v;
istream_iterator<int> in(cin), eof;
while (in!=eof)
    v.push_back(*in++);
```
它等价于
```c++
vector<int> v;
int x;
while (cin >> x)
    v.push_back(x);
```
但其实 `vector` 有一个接受两个·迭代器版本的构造函数，所以我们还可以写成如下形式：
```c++
istream_iterator<int> in(cin), eof;
vector<int> v(in, eof);
```
这体现了流迭代器的简便之处。

下表列出了输入流迭代器支持的操作（`ii<T>` 表示 `istream_iterator<T>`）：
|||
|---|---|
|`ii<T> in(is);`|`in` 从输入流 `is` 读入类型为 `T` 的值|
|`ii<T> end;`|读入类型为 `T` 的值的输入流迭代器，表示尾后位置|
|`in1 == in2`|`in1` 和 `in2` 必须读取相同类型。如果它们绑定了同一个流，或都为尾后位置则为真|
|`in1 != in2`|见 `in1 == in2`|
|`*in`|返回从流中读取的值|
|`++in, in++`|使用元素定义的运算符 `>>` 读取下一个值|

我们还可以使用任何传入输入迭代器的标准库算法，下面是一个例子：
```c++
istream_iterator<int> in(cin), eof;
cout << accumulate(in, eof, 0) << endl;
```
其中 `accumulate` 函数负责求和。在这里，它表示 `0` 加上从 `in` 到 `eof` 之前（即在文件尾或非法输入之前输入的 `int`）的和。

如输入：
`1 2 3 4 5 6 7 8 9`

则输出 `45`。

### `ostream_iterator`

输出流迭代器类似输入，但不支持尾后元素（即默认构造）。创建一个输出流迭代器时，提供一个可选的第二参数，它是一个字符串，表示每次输出后打印的字符串。它必须是一个 `C` 风格字符串。

输出流迭代器支持的所有操作如下表（其中 `oi<T>` 表示 `ostream_iterator<T>`）：

|||
|---|---|
|`oi<T> out(os)`|`out` 将类型为 `T` 的值输出到 `os` 中|
|`oi<T> out(os,d)`|同上，但每次输出后都再输出一个 `d`，`d` 是一个 `C` 风格字符串|
|`out = val`|用 `<<` 运算符将 `val` 写入到 `out` 绑定的流中|
|`*out, ++out, out++`|这些运算符是存在的，但不对 `out` 做任何事情。每个运算符都返回 `out`|

其中最后一条是为了更好的写出泛型代码，就如同之前提到的反向迭代器的递增操作。

```c++
ostream<int> out(cout," "); //注意不要传入字符！
for (auto i : v) //v 是一个 vector
    *out++ = i; //等价于 out = i，但这样的操作更普遍、更易懂
```
同样的，输出流迭代器也有更好的使用方法，那就是把循环替换为标准库算法：

`copy(v.begin(), v.end(), out);`

### 例题：[A+B Problem](https://www.luogu.com.cn/problem/P1001)

让我们用流迭代器解决解决这个问题[（或成为全谷最臭代码](https://www.luogu.com.cn/record/55312776)：

```c++
#include <iostream>
#include <iterator>
#include <algorithm>
using namespace std;

int main()
{
    ostream_iterator<int> out(cout,"\n");
    istream_iterator<int> in(cin);
    int a = *in++;
    int b = *in++;
    out = a + b;
    return 0;
}
```
您也可以尝试使用前文提到的 `accumulate` 函数（需要头文件 `numeral`）来收获更短的代码，或改用 `cout` 来收获更更短的代码。

## 3. 插入迭代器

插入迭代器是一个很神奇的东西。例如下面的代码
```c++
vector<int> v1 = {0, 1, 2, 3};
vector<int> v2(v1.size());
copy(v1.begin(), v1.end(), v2.begin());
```
实际上可以简化成如下代码：

```c++
vector<int> v1 = {0, 1, 2, 3};
vector<int> v2;
copy(v1.begin(), v2.end(), back_inserter(v2));
```

不难发现，`back_inserter` 返回一个迭代器，但它本身并不是一个迭代器。实际上，“插入迭代器”都是**迭代器适配器**（相关概念请自行 BFS）。

下表列出了插入迭代器返回的迭代器的操作：

|||
|---|---|
|`it = t`|在 `it` 的指定位置插入元素|
|`*it, ++it, it++`|这些运算符是存在的，但不对 `it` 做任何事情。每个运算符都返回 `it`|

对于“指定位置”有：

- `front_inserter` 等价于调用 `push_front`;

- `back_inserter` 等价于调用 `push_back`;

- `inserter` 接受一个第二参数，它是一个迭代器 `it`，等价于调用 `insert`，且插入位置在 `it` 的前面。

```c++
// 三种 inserter 的示例

// 输出：
// 5 1 2 3 4
// 4 3 2 1 5
// 1 2 3 4 5

#include <iostream>
#include <list>
#include <algorithm>
#include <iterator>
using namespace std;
void print(list<int> l)
{
    ostream_iterator<int> out(cout, " ");
    copy(l.begin(), l.end(), out);
    cout << endl;
}
int main()
{
    list<int> l = {1, 2, 3, 4};
    list<int> l1 = {5}, l2 = {5}, l3 = {5}; // 空 list
    copy(l.begin(), l.end(), back_inserter(l1));
    copy(l.begin(), l.end(), front_inserter(l2));
    copy(l.begin(), l.end(), inserter(l3, l3.begin())); // 在 l3.begin() 前面插入
    print(l1);
    print(l2);
    print(l3);
    return 0;
}
```
实际上，这些迭代器也可以存下来：

`auto bit=back_inserter(v);`

同时，只有支持对应操作的容器才能使用。

## 4. 参考文献

*C++ Primer 5th edition*