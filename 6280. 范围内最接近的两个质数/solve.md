# 线性筛
## 方法一(线性筛)：
### 思路
1. 通过线性筛的方法把所有小于等于$right$的质数求出
2. 然后从$>=left$的最小质数开始遍历求出的质数中，满足$nums2 - nums1$的最小值
### 复杂度
- 时间复杂度:
  > $O(n+logk+k)$，n为right的大小， k为小于等于right的所有质数，logk为二分查找时间
- 空间复杂度:
  > $O(n)$，存储质数跟判别质数的数组大小

### Code
```C++ []
class Solution {
public:
    vector<int> closestPrimes(int left, int right) {
        vector<int> primes;
        vector<int> isPrime(right + 1, 1);

        for (int i = 2;i <= right;i++) {
            if (isPrime[i]) primes.push_back(i);

            for (int j = 0;j < primes.size() && i * primes[j] <= right;j++) {
                isPrime[i * primes[j]] = 0;
                if (i % primes[j] == 0) break;
            }
        }

        vector<int> ans = { -1,-1 };
        if (primes.size() == 0) return ans;
        int begin = lower_bound(primes.begin(), primes.end(), left) - primes.begin();
        int maxD = right + 1;
        for (;begin < primes.size() - 1;++begin) {
            int curD = primes[begin + 1] - primes[begin];
            if (curD < maxD) {
                maxD = curD;
                ans[0] = primes[begin];
                ans[1] = primes[begin + 1];
            }
        }
        return ans;
    }
};
```