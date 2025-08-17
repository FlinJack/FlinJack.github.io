---
title: "0-1 背包问题及动态规划题解"
category: algorithms
tags: [动态规划, 算法, 0-1背包]
permalink: /notes/2025-08-13-0-1-knapsack-problem
excerpt: "介绍经典动态规划问题——0-1 背包问题，包含二维、一维状态转移代码示例。"
date: 2025-08-13
---

## 1. 题目介绍

有 N 件物品和一个容量为 V 的背包，每件物品有各自的价值且只能被选择一次，要求在有限的背包容量下，装入的物品总价值最大。

「0-1 背包」是较为简单的动态规划问题，也是其余背包问题的基础。

动态规划是不断决策求最优解的过程，「0-1 背包」即是不断对第 i 个物品做出决策，「0-1」正好代表不选与选两种决定。

---

## 2. 题解代码（C++）

### 2.1 版本1：二维数组

- 状态 f[i][j] 定义：前 i 个物品，背包容量 j 下的最优解（最大价值）。
- 递推关系：
  - 如果当前背包容量 j 不够装第 i 件物品，则 f[i][j] = f[i-1][j]
  - 否则 f[i][j] = max(f[i-1][j], f[i-1][j - v[i]] + w[i])

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1005;
int v[MAXN];    // 体积
int w[MAXN];    // 价值
int f[MAXN][MAXN];  // f[i][j] 表示前 i 个物品，容量为 j 的最大价值

int main() {
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];

    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++) {
            if(j < v[i])
                f[i][j] = f[i-1][j];
            else
                f[i][j] = max(f[i][j], f[i-1][j - v[i]] + w[i]);
        }

    cout << f[n][m] << endl;
    return 0;
}
```

### 2.2 版本2：一维数组优化

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1005;
int v[MAXN];    // 体积
int w[MAXN];    // 价值
int f[MAXN];    // f[j] 表示容量为 j 的最大价值

int main() {
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
        cin >> v[i] >> w[i];

    for(int i = 1; i <= n; i++)
        for(int j = m; j >= v[i]; j--) {
            f[j] = max(f[j], f[j - v[i]] + w[i]);
        }

    cout << f[m] << endl;
    return 0;
}
```

## 3. 总结

0-1背包问题是动态规划的经典入门题目，通过状态转移方程可以很好地理解动态规划的思想。二维版本更直观，一维版本更高效。
