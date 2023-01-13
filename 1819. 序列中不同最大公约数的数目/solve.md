# 最大公约数特性+枚举
## 方法一(最大公约数特性+枚举)：
### 思路
1. 数组中最大公约数不可能超过数组中的最大值，那么我们逆向思维，来构建成最大公约数的子序列，可以从从$1$枚举到最大值：
   - 假设当前枚举到的最大公约数为$x$，那么数组中能构成最大公约数为$x$的子序列一定是$x$的倍数，我们只要在数组中找到$x$的倍数的最大公约数等于$x$就说明能构成最大公约数为$x$的子序列存在
   - 具体操作可以以当前遍历到的最大公约数$x$为基础，按照倍数增加来查找是否在数组中，如果存在求出最大公约数看是否等于$x$

### 复杂度
- 时间复杂度:
  > $O(n+max(nums)*log(max(nums)))$，开始求出最大值需要$O(n)$，遍历最大公约数每次操作需要$\frac{max(nums)}{x}$，因为$n+\frac{n}{2}+\frac{n}{3}+...+1≈nlogn$，所以最终操作为$max(nums)*log(max(nums))$
- 空间复杂度:
  > $O(max(nums))$，我么需要$max(nums)$个空间来记录数组元素是否出现过

### Code
```C++ []
class Solution {
public:
    int countDifferentSubsequenceGCDs(vector<int>& nums) {
        int maxVal = *max_element(nums.begin(), nums.end());
        vector<int> check(maxVal + 1, false);
        for (auto&& num : nums) check[num] = true;

        int ans = 0;
        for (int i = 1;i <= maxVal;++i) {
            int x = 0;
            for (int j = i;j <= maxVal;j += i) {
                if (check[j]) {
                    if (x == 0) x = j;
                    else x = gcd(x, j);

                    if (x == i) {
                        ++ans;
                        break;
                    }
                }

            }
        }
        return ans;
    }
};
```