# 广度优先搜索
## 方法一(广度优先搜索)：
### 思路
1. 求最短路径可以想到广搜，用$visit$数组标记是否访问过，如果访问过表示已经有之前路径已经是最优了

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为矩阵$grid$行列数，最坏情况下，矩阵中全为$0$
- 空间复杂度:
  > $O(n^2)$，$visit$数组大小，队列不会超过$n^2$

### Code
```C++ []
class Solution {
public:
    static constexpr int dirs_8[8][2] = { {-1, 0}, {1, 0}, {0, -1}, {0, 1}, {-1,1}, {-1,-1}, {1,-1}, {1,1} };

    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int n = grid.size();
        if (grid[0][0] || grid[n - 1][n - 1]) return -1;

        vector<vector<int>> visited(n, vector<int>(n, 0));
        queue<pair<int, int>> q;
        q.emplace(0, 0);
        visited[0][0] = 1;
        int ans = 0;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0;i < size;++i) {
                auto [x, y] = q.front();
                q.pop();
                if (x == n - 1 && y == n - 1) return ans + 1;

                for (int i = 0;i < 8;++i) {
                    int cx = x + dirs_8[i][0], cy = y + dirs_8[i][1];
                    if (cx >= 0 && cx < n && cy >= 0 && cy < n && !visited[cx][cy] && grid[cx][cy] == 0) {
                        q.emplace(cx, cy);
                        visited[cx][cy] = 1;
                    }
                }
            }
            ++ans;
        }

        return -1;
    }
};
```