# 排序+模下快速幂
## 方法一(排序+模下快速幂)：
### 思路
1. 由于题目给定，两个交集区间只能在一个组内，我们可以先把没有交集的区间求出，看能分成多少组。

2. 区间求组可以先排序，这样区间就按照从小到大的顺序排列开来，可以求出没有交集的组的个数$group$，那么排列的总方案数就为$2^{group}$

3. 由于方案数很大需要取模，我们可以用模下快速幂求出最终答案。[快速幂](https://oi-wiki.org/math/binary-exponentiation/)


### 复杂度
- 时间复杂度:
  > $O(nlogn+log(group))$，$n$为数组$ranges$的大小，排序需要$O(nlogn)$，快速幂复杂度为$O(log(group))$
- 空间复杂度:
  > $O(logn)$，排序需要栈空间

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

    int countWays(vector<vector<int>>& ranges) {
        sort(ranges.begin(), ranges.end());
        int pre = -1, group = 0;
        for (auto&& x : ranges) {
            if (pre < x[0]) ++group;
            pre = max(pre, x[1]);
        }

        return binpow(2, group, MOD);
    }
};
```