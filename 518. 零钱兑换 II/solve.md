# 动态规划
## 方法一（动态规划）
### 思路
1. 求提供的$coins$能组成$amount$的组合方法
2. 设$dp[i]$表示由$coins$组成金币$i$的组合法，那么$dp[amount]$就是我们要求的。边界条件，当$i=0$时，表示用任何$coins$都组成不了$0$，任何$coins$都不选的方法只有一种，即$dp[0]=1$
3. 我们通过遍历$coins$，计算出它的贡献值，例如：
```
amount = 5, coins = [1, 2, 5]
coin=1时，
dp[1]=dp[1]+dp[1-1]=1   [1]
dp[2]=dp[2]+dp[2-1]=1   [1,1]
dp[3]=dp[3]+dp[3-1]=1   [1,1,1]
dp[4]=dp[4]+dp[4-1]=1   [1,1,1,1]
dp[5]=dp[5]+dp[5-1]=1   [1,1,1,1,1]
coin=2时，
dp[2]=dp[2]+dp[2-2]=2   [1,1]+[2]
dp[3]=dp[3]+dp[3-2]=2   [1,1,1]+[1,2]
dp[4]=dp[4]+dp[4-2]=3   [1,1,1,1]+[1,1,2]+[2,2]
dp[5]=dp[5]+dp[5-2]=3   [1,1,1,1,1]+[1,1,1,2]+[1,2,2]
coin=5时，
dp[5]=dp[5]+dp[5-5]=4   [1,1,1,1,1]+[1,1,1,2]+[1,2,2]+[5]
```
得到状态转移方程：
$dp[i]+=dp[i-coin]，i\in[coin,amount]$

因为遍历次序是固定的，所以能保证转移是正确的，不会出现重复计算

### 复杂度
- 时间复杂度:
  > $O(amount*n)$，其中$amount$为金额，$n$为$coins$数量
- 空间复杂度:
  > $O(amount)$

### Code
```C++ []
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1);
        dp[0] = 1;
        for (auto&& coin : coins) {
            for (int i = coin;i <= amount;++i) {
                dp[i] += dp[i - coin];
            }
        }
        return dp[amount];
    }
};
```