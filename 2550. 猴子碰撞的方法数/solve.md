# 数论+快速幂
## 方法一(数论+快速幂)：
### 思路
1. 题目描述有问题，最终是到达同一点或者移动过程中相向都表示为相撞，那么不相撞只有两种情况，全部顺时针移动或者全部逆时针移动。因为每个位置可以前后移动，总共$2^n$种排列方式，最终相撞有$2^n-2$种排列方式。

2. 求幂的过程因为要求模，所以可用模下快速幂完成。

### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为给定数，快速幂需要$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    static const int MOD = 1e9 + 7;

    long long binpow(long long a, long long b, long long m) {
        a %= m;
        long long res = 1;
        while (b) {
            if (b & 1) res = res * a % m;
            a = a * a % m;
            b >>= 1;
        }
        return res;
    }

    int monkeyMove(int n) {
        return (binpow(2, n, MOD) - 2 + MOD) % MOD;
    }
};
```