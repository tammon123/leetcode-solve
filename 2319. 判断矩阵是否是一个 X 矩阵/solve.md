# 模拟
## 方法一(模拟)：
### 思路
1. 由题目知，我们只需要遍历矩阵，对角线上的点如果为$0$或者不再对角线上的点不为$0$我们都返回$false$，否则遍历结束满足条件为$X$矩阵

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为矩阵$grid$行列大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool checkXMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        for (int i = 0;i < n;++i) {
            if (grid[i][i] == 0 || grid[i][n - i -1] == 0) return false;
            for (int j = 0;j < n;++j) {
                if (j != i && j != n - i - 1 && grid[i][j] != 0) return false;
            }
        }
        return true;
    }
};
```
