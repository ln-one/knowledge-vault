---
tags:
  - Knowledge/Algorithm
  - Knowledge/Algorithms/Math
created: 2025-09-18
author:
  - ln1
status: Done
---
## Description

有 N 头奶牛围成一个圈，每头奶牛抽取一个整数 Ai。当轮到奶牛 i时，她会拍所有其他奶牛 j的头，只要 Aj能整除 Ai。请计算每头奶牛拍了多少下其他奶牛的头。
### Example:  
Input: 
```
5 
2 
1 
2 
3 
4 

```

Output: 
```
2 
0 
2 
1 
3 

```
## Question Link

[P2926 [USACO08DEC] Patting Heads S - 洛谷](https://www.luogu.com.cn/problem/P2926)
## Approach

- **统计每个数字出现的次数** 用一个数组 `cnt[x]` 表示数字 x 出现了多少次。
    
- **枚举每个数字的倍数** 对于每个数字 i，它会对所有是它倍数的数字 j 产生贡献（即：如果某奶牛拿的是 j，而 i 是 j 的约数，那么拿 j 的奶牛会被拍头）。
    
    所以我们可以用一个数组 `w[j]` 表示有多少奶牛的数字能整除 j。
    
- **计算每头奶牛的拍头次数** 对于每头奶牛拿的数字 a[i]，她拍头的次数就是 `w[a[i]] - 1`（减去自己）。


## Implementation

```cpp
#include <iostream>

using namespace std;

const int MAXN = 1e6 + 10;
int n, a[MAXN], cnt[MAXN], w[MAXN];

int main() {
  cin >> n;

  for (int i = 1; i <= n; i++) {
    cin >> a[i];
    cnt[a[i]]++;
  }

  for (int i = 1; i < MAXN; i++) {
    for (int j = i; j < MAXN; j += i) {
      w[j] += cnt[i];
    }
  }

  for (int i = 1; i <= n; i++) {
    cout << w[a[i]] - 1 << endl;
  }
  return 0;
}

```

## Analysis
- 今天偏头痛，随便做一做。