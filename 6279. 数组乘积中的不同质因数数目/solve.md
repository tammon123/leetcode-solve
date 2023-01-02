# 哈希表+质因数分解
## 方法一(哈希表+质因数分解)：
### 思路
1. 求质因数方法可用质因数分解方法，同[2507. 使用质因数之和替换后可以取到的最小值](https://leetcode.cn/problems/smallest-value-after-replacing-with-sum-of-prime-factors/)
2. 遍历数组，把所有质因数都存入哈希表中，防止重复，最后统计哈希表大小
### 复杂度
- 时间复杂度:
  > $O(n*x^{\frac{1}{4}})$，n为num的位数，$x^{\frac{1}{4}}$为Pollard Rho算法复杂度
- 空间复杂度:
  > $O(nlogx+k)$，nlogx质因数分解所需栈空间，k为哈希表存储的质数个数，题目最大数字为1000,k最大168

### Code
```C++ []
class Solution {
public:
    int distinctPrimeFactors(vector<int>& nums) {
        unordered_set<int> us;
        auto get = [&](int x, auto&& get) ->void {
            for (int k = 2;;++k) {
                if (k == x) {
                    us.emplace(k);
                    break;
                }
                if (x > k && x % k == 0) {
                    us.emplace(k);
                    x /= k;
                    get(x, get);
                    break;
                }
            }
        };

        for (auto&& x : nums) get(x, get);
        return us.size();
    }
};
```