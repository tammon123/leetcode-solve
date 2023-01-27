# 动态规划/记忆化搜索
## 方法一(动态规划)：
### 思路
1. 假设$dp[i]$表示能组成总金额$i$的最少硬币数。那么当前总金额$i$是由上一总金额$j$加上一个硬币面额组成，也就是说当前状态由当前总金额减去每一个硬币面额的总金额组成的硬币数的最小值加一转移而来，即$dp[i]=min(dp[i-coins[j]])+1$，注意当前总金额数需要大于等于硬币面额，即:$i>=coins[j]$

2. 边界条件，总金额为$0$，所需硬币数为$0$，有$dp[0]=0$

### 复杂度
- 时间复杂度:
  > $O(kn)$，$n$为硬币数，$k$为总金额，没次总金额状态都需要枚举一遍面额值，所以需要$O(kn)$
- 空间复杂度:
  > $O(k)$，需要存储一个长为$k$的状态数组

### Code
```C++ []
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 1;i <= amount;++i) {
            for (auto&& coin : coins) {
                if (coin <= i) {
                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
};
```

## 方法二(记忆化搜索)：
### 思路
1. 假设组成当前总金额的最小硬币数为$cnt[i]$，那么组成当前总金额最后需要的硬币为$c$，那么有转移方程:$cnt[i]=cnt[i-c]+1$。但是我们不知道最后所需的硬币是多少，所以需要遍历硬币，取其中的最小值，所以有$cnt[i]=\min\limits_{0<=j<n}cnt[i-c_j]+1，i>=c_j$，边界条件有$cnt[0]=0$

2. 可用记忆化搜索，避免重复计算，记录$cnt$值

### 复杂度
- 时间复杂度:
  > $O(kn)$，$n$为硬币数，$k$为总金额，没次总金额状态都需要枚举一遍面额值，所以需要$O(kn)$
- 空间复杂度:
  > $O(k)$，需要存储一个长为$k$的状态数组

### Code
```C++ []
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int> meme(amount + 1);
        auto dfs = [&](int need, auto&& dfs)->int {
            if (need < 0) return -1;
            if (need == 0) return 0;
            if (meme[need]) return meme[need];
            int cnt = INT_MAX;
            for (auto&& coin : coins) {
                int ret = dfs(need - coin, dfs);
                if (ret >= 0 && ret < cnt) {
                    cnt = ret + 1;
                }
            }

            return meme[need] = cnt == INT_MAX ? -1 : cnt;
        };
        return dfs(amount, dfs);
    }
};
```