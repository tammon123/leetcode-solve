# 二维前缀和
## 方法一(二维前缀和)：
### 思路
1. 区域和可用二维前缀和的想法。有：
   
   ```P[i][j] = P[i - 1][j] + P[i][j - 1] - P[i - 1][j - 1] + A[i][j]```

2. 然后我们枚举正方形边长，最长为$k=min(m,n)$其中$m,n$为矩阵的行列。判断每个边长为$k$大小的区域和是否小于等于阈值，求出最大$k$
### 复杂度
- 时间复杂度:
  > $O(mnk)$，$m,n$为矩阵的行列，$k=min(m,n)$
- 空间复杂度:
  > $O(mn)$，二维前缀和需要的空间

### Code
```C++ []
class Solution {
public:
    int maxSideLength(vector<vector<int>>& mat, int threshold) {
        int m = mat.size(), n = mat[0].size();
        vector<vector<int>> prefixsum(m + 1, vector<int>(n + 1));
        int ans = 0;
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                prefixsum[i][j] = prefixsum[i - 1][j] + prefixsum[i][j - 1] - prefixsum[i - 1][j - 1] + mat[i - 1][j - 1];
                for (int k = ans + 1;i - k >= 0 && j - k >= 0;++k) {
                    int s1 = prefixsum[i][j] - prefixsum[i - k][j] - prefixsum[i][j - k] + prefixsum[i - k][j - k];
                    if (s1 <= threshold) ++ans;
                }
            }
        }

        return ans;
    }
};
```