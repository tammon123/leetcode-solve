# 模拟(迭代递归过程)
## 思路
1. 每次求出质因数的和，然后迭代
2. 质因数可用[Pollard Rho因数分解](https://baike.baidu.com/item/%E8%B4%A8%E5%9B%A0%E6%95%B0/6192269)

## 复杂度
- 时间复杂度:
  > $O(x^{\frac{1}{4}}*n)$，$x^{\frac{1}{4}}$为Pollard Rho算法复杂度，n数字大小
- 空间复杂度:
  > $O(n*logx)$，logx为递归所需栈空间

## Code
```C++ []
class Solution {
public:
    int smallestValue(int n) {
        auto getsum = [&](int x, auto&& getsum) ->int {
            int ans = 0;
            for (int k = 2;;++k) {
                if (k == x) {
                    ans += k;return ans;
                }
                if (x > k && x % k == 0) {
                    ans += k;
                    x /= k;
                    return ans + getsum(x, getsum);
                }
            }
            return ans;
        };

        while (n) {
            if (n == getsum(n, getsum)) return n;
            else n = getsum(n, getsum);
        }
        return 0;
    }
};
```