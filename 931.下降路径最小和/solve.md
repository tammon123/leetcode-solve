# 动态规划
## 思路
1. 要求下降路径最小和，我们可以转化为每一行的$matrix[i][j]=min(matrix[i-1][j-1], min(matrix[i-1][j], matrix[i-1][j+1])$而来
2. 特别的当$i=0$时，$matrix[i][j]$不变;当$j=0$时，$matrix[i][j]=min(matrix[i-1][j], matrix[i-1][j+1])$;当$j=n-1$时，$matrix[i][j]=min(matrix[i-1][j-1], matrix[i-1][j])$

## Code
```C++[]
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size(), ans = INT_MAX;
        for (int i = 0;i < n;++i) {
            for (int j = 0;j < n;++j) {
                if (i > 0) {
                    if (j == 0) matrix[i][j] = min(matrix[i - 1][j], matrix[i - 1][j + 1]) + matrix[i][j];
                    else if (j == n - 1) matrix[i][j] = min(matrix[i - 1][j - 1], matrix[i - 1][j]) + matrix[i][j];
                    else matrix[i][j] = min(min(matrix[i - 1][j - 1], matrix[i - 1][j]), matrix[i - 1][j + 1]) + matrix[i][j];
                }
                if (i == n - 1) ans = min(ans, matrix[i][j]);
            }
        }
        return ans;
    }
};
```