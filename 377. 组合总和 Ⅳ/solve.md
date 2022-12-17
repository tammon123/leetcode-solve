# 动态规划
## 思路
1. 类似零钱兑换II
2. 设$dp[i]$为和为$i$的**排列**个数，$dp[target]$为所求解。假设$1\le\text{i}\le\text{target}$时，一定存在一种排列其最后一个元素为$num$，使得$i-num$之前的排列和加上$num$之后等于$i$

所以转移公式有：
$\left\{
    \begin{array} {l}
        dp[0]=1，&i=0 \\
        dp[i]+=\sum\limits_{nums}^{nums}dp[i-num]，&1\le\text{i}\le\text{target}
    \end{array}
\right.
$

### 复杂度
- 时间复杂度:
  > $O(target*n)$，n为nums的长度
- 空间复杂度:
  > $O(target)$

### Code
```C++ []
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1);
        dp[0] = 1;
        for (int i = 1;i <= target;++i) {
            for (auto&& num : nums) {
                if (num <= i && dp[i - num] < INT_MAX - dp[i]) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
};
```