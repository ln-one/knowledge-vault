---
tags:
  - Knowledge/Algorithm
  - Knowledge/Algorithms/GCD
created: 2025-09-23
author:
  - ln1
status: In Progress
---
## Description

给定四个正整数，求满足特定最大公约数和最小公倍数条件的正整数 (x) 的个数。
### Example:  
Input: 
```
2 
41 1 96 288 
95 1 37 1776 
```

Output: 
```
6 
2
```
## Question Link

[P1072 [NOIP 2009 提高组] Hankson 的趣味题 - 洛谷](https://www.luogu.com.cn/problem/P1072)
## Approach

### 核心思路
- 条件：
  $$
  \gcd(x, a_0) = a_1,\quad \mathrm{lcm}(x, b_0) = b_1
  $$
- 推导：
  $$
  a_1 \mid x,\quad x \mid b_1
  $$
  所以候选解一定是 $b_1$ 的因子，且能被 $a_1$ 整除。

### 化简条件
- 设：
  $$
  p = \frac{a_0}{a_1},\quad q = \frac{b_1}{b_0}
  $$
- 等价判定：
  $$
  \gcd\!\left(\frac{x}{a_1},\, p\right) = 1,\quad \gcd\!\left(\frac{b_1}{x},\, q\right) = 1
  $$

### 解法步骤
1. 枚举 $b_1$ 的所有因子 $d$。
2. 检查是否满足：
   $$
   d \equiv 0 \pmod{a_1},\quad \gcd\!\left(\frac{d}{a_1},\, p\right)=1,\quad \gcd\!\left(\frac{b_1}{d},\, q\right)=1
   $$
3. 统计符合条件的因子个数。

## Implementation

```cpp
#include <iostream>

using namespace std;
typedef long long ll;

ll gcd(ll a, ll b) {
  while (b != 0) {
    ll t = a % b;
    a = b;
    b = t;
  }
  return a;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);
  int n;
  cin >> n;
  ll a0, a1, b0, b1;
  while (n--) {
    cin >> a0 >> a1 >> b0 >> b1;
    ll p = a0 / a1;
    ll q = b1 / b0;
    int ans = 0;
    for (ll i = 1; i * i <= b1; i++) {
      if (b1 % i == 0) {
        ll d1 = i;
        ll d2 = b1 / i;

        if (d1 % a1 == 0 && gcd(d1 / a1, p) == 1 && gcd(b1 / d1, q) == 1) {
          ans++;
        }

        if (d1 != d2 && d2 % a1 == 0 && gcd(d2 / a1, p) == 1 &&
            gcd(b1 / d2, q) == 1) {
          ans++;
        }
      }
    }
    cout << ans << endl;
  }
}

```

## Analysis
[[P1029-最大公约数和最小公倍数问题]]
### 互质条件的数学推理

#### 定义
$$
p = \frac{a_0}{a_1}, \quad q = \frac{b_1}{b_0}
$$

---

#### 1. 关于 $\gcd\!\left(\tfrac{x}{a_1},\, p\right)=1$ 的必要性

已知 $\gcd(x, a_0) = a_1$。因为 $a_1 \mid x$ 且 $a_1 \mid a_0$，设
$$
x = a_1 \cdot t, \quad a_0 = a_1 \cdot p
$$

于是：
$$
\gcd(x, a_0) = \gcd(a_1 t,\, a_1 p) = a_1 \cdot \gcd(t, p)
$$

要使其等于 $a_1$，必须有 $\gcd(t, p) = 1$。  
等价于：
$$
\gcd\!\left(\frac{x}{a_1},\, p\right) = 1
$$

---

#### 2. 关于 $\gcd\!\left(\tfrac{b_1}{x},\, q\right)=1$ 的必要性

已知 $\operatorname{lcm}(b_0, x) = b_1$。因为 $x \mid b_1$，设
$$
b_1 = b_0 \cdot q
$$

对任一质因子 $\pi$，有
$$
\nu_{\operatorname{lcm}(b_0,x)}(\pi) = \max(\nu_{b_0}(\pi), \nu_x(\pi))
$$
而
$$
\nu_{b_1}(\pi) = \nu_{b_0}(\pi) + \nu_q(\pi)
$$

要使两者相等，凡是 $\nu_q(\pi) > 0$ 的质因子，其增量必须完全由 $x$ 提供。  
这等价于：
$$
\gcd\!\left(\frac{b_1}{x},\, q\right) = 1
$$

---

#### 3. 总结

- $\gcd\!\left(\tfrac{x}{a_1},\, p\right)=1$ 保证 gcd 不会比 $a_1$ 更大。  
- $\gcd\!\left(\tfrac{b_1}{x},\, q\right)=1$ 保证 lcm 不会比 $b_1$ 更小。  

这两个条件正好把 gcd 和 lcm 的约束“卡死”，既不多也不少。
