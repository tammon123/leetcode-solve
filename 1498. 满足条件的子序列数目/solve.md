# 排序+快速幂+二分查找
## 方法一(排序+快速幂+二分查找)：
### 思路
1. 首先求的是子序列数目，满足子序列最小值与最大值的和小于等于$target$，那么整个子序列的顺序无论怎样都不会影响最终结果，所以方便后面的查找，我们可以对数组先进行排序。

2. 假如当前的数为$x$，从这位开始往后找到$y$使得$x+y<=target$，那么包含$x,y$的区间值都会对答案子序列贡献数量。具体贡献数量是一个等比数列加$2*x$是否小于等于$target$，满足就加一，否则就是等比数列。例如：
```
[3,5,6,7] target=9
x=3, y=6, 子序列数量=2^(2-0)-1+2*3<=9=4 即：[3,3] [3,5] [3,6] [3,5,6] 往后数字没有满足条件的
又例如:
[2,3,3,4,6,7] target=12
x=2, y=7 子序列数量=2^(5-0)-1+(2*2<=12)=32
x=3, y=7 子序列数量=2^(5-1)-1+(2*3<=12)=16
x=3, y=7 子序列数量=2^(5-2)-1+(2*3<=12)=8
x=4, y=7 子序列数量=2^(5-3)-1+(2*4<=12)=4
x=6, y=6 子序列数量=2^(4-4)-1+(2*6<=12)=1
x=7 不满足
最终满足条件的子序列数量为： 32+16+8+4+1=61
```
3. 用二分法找到$y>target+x$的位置$j$，那么等比数量求和公式为:$\sum(\frac{1-2^{j-i-1}}{1-2})=\sum(2^{j-i-1}-1)$

4. 由于幂的值会很大，我们可以用[模下快速幂](https://oi-wiki.org/math/binary-exponentiation/)来求
### 复杂度
- 时间复杂度:
  > $O(nlog^2n)$，$n$是数组$nums$的大小，排序需要$O(nlogn)$，遍历数组需要$O(n)$，每次查询二分需要$O(logn)$，快速幂为$O(logk)$，最坏情况下每次是最长串，趋于$O(logn)$
- 空间复杂度:
  > $O(logn)$，排序所需要空间

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
    int numSubseq(vector<int>& nums, int target) {
        int n = nums.size(), ans = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0;i < n;++i) {
            int j = upper_bound(nums.begin() + i, nums.end(), target - nums[i]) - nums.begin();
            int b = max(j - i - 1, 0);
            ans = (long long)(ans + binpow(2, b, MOD) - 1 + (2 * nums[i] <= target)) % MOD;
        }
        return ans;
    }
};
```