# 动态规划/组合数学
## 方法一(动态规划)：
### 思路
1. 因为每一步只能向右或者向下，那么假设从左上角走到当前点的路径数量为$dp[i][j]$，那么状态转移方程有$dp[i][j]=dp[i-1][j]+dp[i][j-1]$
2. 我们考虑边界，初始点路径为$dp[0][0]=1$，即：从开始点到开始点路径为$1$，那么$dp[0][j]$与$dp[i][0]$只有一条路径，全都置为$1$
3. 因为考虑$i-1$行都是由$i$行转移而来，那么我们可以状压，把二维压为一维数组。并且交换行列对结果并不会产生影响，我么可以始终让$n<m$，最终空间复杂度可以为$O(min(m,n))$

### 复杂度
- 时间复杂度:
  > $O(mn)$
- 空间复杂度:
  > $O(O(min(m,n)))$

### Code
```C++ []
class Solution {
public:
    int uniquePaths(int m, int n) {
        if (m < n) return uniquePaths(n, m);
        vector<int> ans(n, 1);
        for (int i = 1;i < m;++i) {
            for (int j = 1;j < n;++j) {
                ans[j] = ans[j - 1] + ans[j];
            }
        }
        return ans[n - 1];
    }
};
```

## 方法二(组合数学)：
### 思路
1. 因为从坐上到右下，需要移动$m+n-2$步，其中向右移动$n-1$，向下移动$m-1$，那么路径总数就是从$m+n-2$次移动中选择向下的$m-1$次移动方案：
   $C_{m+n-2}^{m-1}=\frac{(m+n-2)*(m+n-1)\cdots n}{(m-1)!}$

### 复杂度
- 时间复杂度:
  > $O(min(m,n))$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int uniquePaths(int m, int n) {
        if (m > n) return uniquePaths(n, m);
        long long ans = 1ll;
        for (int i = 1, j = n;i < m;++i, ++j) {
            ans = ans * j / i;
        }
        return ans;
    }
};
```