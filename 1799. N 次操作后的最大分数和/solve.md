# 状压+记忆化搜索
## 方法一(状压+记忆化搜索)：
### 思路
1. 我们需要$n$次操作，每次选$2$个不同$gcd$组合，因为最多$7$次操作，也就是$14$个数据，那我们只需要构建二维数组来预处理$gcd$组合
2. 我们可用状态压缩思想，把每次组合压缩成一个数字，例如$3$的二进制表示为``011``，也就是说选择了$nums[1]、nums[0]$，通过记忆化搜索方法，自上而下的搜索所有组合的最大值，最终$(1<<n)-1$为我们求的最终最大组合数
3. 裁剪过程，把要选择的位置置为$0$，选择$1$的继续搜索，即：$mask\oplus(1<<i)\oplus(1<<j)$，再遍历过程$mask$二进制位为$0$表示选择过了，可直接跳过

### 复杂度
- 时间复杂度:
  > $O(n^2+n^2logn)$，n为数组大小
- 空间复杂度:
  > $O(n^2+logn)$

### Code
```C++ []
class Solution {
public:
    int maxScore(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0;i < n;++i) {
            for (int j = i + 1;j < n;++j) {
                gcds[i][j] = __gcd(nums[i], nums[j]);
            }
        }
        vector<int> memo(1 << n, 0);

        auto dfs = [&](int state, int ops, auto&& dfs) ->int {
            if (memo[state]) return memo[state];
            for (int i = 0;i < n;++i) {
                for (int j = i + 1;j < n;++j) {
                    if (((state >> i) & 1) == 0 || ((state >> j) & 1) == 0) continue;
                    int next_state = state ^ (1 << i) ^ (1 << j);
                    memo[state] = max(memo[state], dfs(next_state, ops + 1, dfs) + ops * gcds[i][j]);
                }
            }
            return memo[state];
        };

        return dfs((1 << n) - 1, 1, dfs);
    }
private:
    int gcds[15][15];
};
```