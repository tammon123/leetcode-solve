# 二维前缀和/动态规划
## 方法一(二维前缀和)：
### 思路
1. 在矩阵上想着正方形区域，因为只要满足边长上都是$1$，那么我们可以求出整个正方形区域和减去中间正方形区域和就可以得到边长的和。

2. 区域和可以考虑二维前缀和，二维前缀和可以参考[304. 二维区域和检索 - 矩阵不可变](https://leetcode.cn/problems/range-sum-query-2d-immutable/)

3. 假设当前点为$grid[i-1][j-1]$(减一方便前缀和表示)，遍历到的正方形边长为$k$，那么边长和可以表示为:
   
   $s1=pre[i][j]-pre[i-k][j]-pre[i][j-k]+pre[i-k][j-k]$

   $s2=pre[i-1][j-1]-pre[i-k+1][j]-pre[i][j-k+1]+pre[i-k+1][j-k+1]$

   边长和表示为$s1-s2$，由于边长都由$1$组成的和为$k^2-(k-2)^2=4k-4$，那么只要条件判断$s1-s2=4k-4$，整个正方形就是符合条件的正方形，计入答案中。

4. 那么我们只需要一边求前缀和一边求出最大以$1$为边界的正方形。

### 复杂度
- 时间复杂度:
  > $O(mnk)$，$m,n$分别为矩阵$grid$的行列，$k$为行列中的最小值
- 空间复杂度:
  > $O(mn)$，前缀和需要的大小

### Code
```C++ []
class Solution {
public:
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> prefixsum(m + 1, vector<int>(n + 1));
        int ans = 0;
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                prefixsum[i][j] = prefixsum[i - 1][j] + prefixsum[i][j - 1] - prefixsum[i - 1][j - 1] + grid[i - 1][j - 1];
                if (grid[i - 1][j - 1] != 1) continue;
                for (int k = 1;i - k >= 0 && j - k >= 0;++k) {
                    int s1 = prefixsum[i][j] - prefixsum[i - k][j] - prefixsum[i][j - k] + prefixsum[i - k][j - k];
                    int s2 = prefixsum[i - 1][j - 1] - prefixsum[i - k + 1][j - 1] - prefixsum[i - 1][j - k + 1] + prefixsum[i - k + 1][j - k + 1];
                    int t = k - 2;
                    if (s1 - s2 == k * k - t * t) ans = max(ans, k * k);
                }
            }
        }
        return ans;
    }
};
```

## 方法二(动态规划)：
### 思路
1. 我们可以用动态规划的思想解决问题，假设$left[i][j]$表示$grid[i-1][j-1]$左边之前连续$1$的数量包含$grid[i-1][j-1]=1$，$up[i][j]$表示$grid[i-1][j-1]$上面之前连续$1$的数量，包含$grid[i-1][j-1]=1$。它们都可以由上一个状态转移而来。

2. 那么当前能组成以$1$为边界的最大正方形边长为$k=min(left[i][j],up[i][j])$，我们需要考虑能满足条件的条件方程为:$left[i-k+1][j]>=k\And\And up[i][j-k+1]>=k$，不满足就减少$k$

### 复杂度
- 时间复杂度:
  > $O(mnk)$，$m,n$分别为矩阵$grid$的行列，$k$为行列中的最小值
- 空间复杂度:
  > $O(mn)$，数组$left,up$需要保存连续$1$的空间数

### Code
```C++ []
class Solution {
public:
    int largest1BorderedSquare(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> left(m + 1, vector<int>(n + 1)), up(m + 1, vector<int>(n + 1));
        int ans = 0;
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                if (grid[i - 1][j - 1] != 1) continue;
                left[i][j] = left[i][j - 1] + 1;
                up[i][j] = up[i - 1][j] + 1;
                int l = min(left[i][j], up[i][j]);
                while (left[i - l + 1][j] < l || up[i][j - l + 1] < l) {
                    --l;
                }
                ans = max(ans, l);
            }
        }
        return ans * ans;
    }
};
```