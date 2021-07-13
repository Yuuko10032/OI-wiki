author: GavinZhengOI, Yuuko10032

## 简介

离散化本质上可以看成是一种 [哈希](../string/hash.md)，其保证数据在哈希以后仍然保持原来的全/偏序关系。

通俗地讲就是当有些数据因为本身很大或者类型不支持，自身无法作为数组的下标来方便地处理，而影响最终结果的只有元素之间的相对大小关系时，我们可以将原来的数据按照从大到小编号来处理问题，即离散化。

用来离散化的可以是大整数、浮点数、字符串等等。

## 实现

C++ 离散化有现成的 STL 算法：

### 离散化数组

将一个数组离散化，并进行查询是比较常用的应用场景：

```cpp
// a[i] 为初始数组,下标范围为 [1, n]
// len 为离散化后数组的有效长度
std::sort(a + 1, a + 1 + n);

len = std::unique(a + 1, a + n + 1) - a - 1;
// 离散化整个数组的同时求出离散化后本质不同数的个数。
```

在完成上述离散化之后可以使用 `std::lower_bound` 函数查找离散化之后的排名（即新编号）：

```cpp
std::lower_bound(a + 1, a + len + 1, x) - a;  // 查询 x 离散化后对应的编号
```

同样地，我们也可以对 `vector` 进行离散化：

```cpp
// std::vector<int> a, b; // b 是 a 的一个副本
std::sort(a.begin(), a.end());
a.erase(std::unique(a.begin(), a.end()), a.end());
for (int i = 0; i < n; ++i)
  b[i] = std::lower_bound(a.begin(), a.end(), b[i]) - a.begin();
```

### 二维离散化

当数据范围较大时，需要将点的坐标进行离散化。下面是一道例题：

???+note " 例题 [[USACO16FEB]Load Balancing S](https://www.luogu.com.cn/problem/P3138)"
    在平面上有 $n(1 \le n \le 1000)$ 个点，坐标为 $(x_i, y_i)(1 \le x_i, y_i \le 10^6)$。要求选择两条垂直于坐标轴的直线，使得划分出的四个区域中点的个数最多的区域中点的个数最少。
    
我们不能直接用一个 $10^6 \times 10^6$ 的二维数组来处理，所以我们需要将点的坐标离散化。具体地，只需要将横坐标和纵坐标分别进行离散化就行了：

```cpp
for (int i = 1; i <= n; ++i) {
    read(x[i]), read(y[i]);
    X[i] = x[i], Y[i] = y[i];
}
std::sort(X + 1, X + n + 1), std::sort(Y + 1, Y + n + 1);
int n1 = std::unique(X + 1, X + n + 1) - X - 1, n2 = std::unique(Y + 1, Y + n + 1) - Y - 1;
for (int i = 1; i <= n; ++i)    x[i] = std::lower_bound(X + 1, X + n1 + 1, x[i]) - X, y[i] = std::lower_bound(Y + 1, Y + n2 + 1, y[i]) - Y;
```
