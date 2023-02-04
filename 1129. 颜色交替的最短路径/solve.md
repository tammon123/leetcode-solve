# 广度优先搜索
## 方法一(广度优先搜索)：
### 思路
1. 求最短路径可以想到广搜，因为要红蓝交替最短，那么$visit$数组要三维数组，前两维表示点$x$到点$y$，后一维表示颜色，即$visit[x][y][z]$。

2. 最开始构图，构建红蓝颜色的有向图。队列可以存储当前点，与上一条路径的颜色，还有步数。假如上一条路径颜色为红色，那么从蓝色有向图中，有从当前点出发的路径，我们存入队列并把步数加一。这样每个点的最短颜色交替路径都能求出。

### 复杂度
- 时间复杂度:
  > $O(n+r+b)$，$n$给定的点数，$r,b$分别为红蓝边的大小
- 空间复杂度:
  > $O(n^2+r+b)$，$visit$数组大小，队列存储点数不会超过$n^2$，保存红蓝边有向图需要$O(r+b)$大小

### Code
```C++ []
class Solution {
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>>& redEdges, vector<vector<int>>& blueEdges) {
        vector<vector<int>> grid_red(n), grid_blue(n);
        for (auto&& edge : redEdges) {
            grid_red[edge[0]].emplace_back(edge[1]);
        }
        for (auto&& edge : blueEdges) {
            grid_blue[edge[0]].emplace_back(edge[1]);
        }

        queue<tuple<int, int, int>> q; //point,color,step
        vector<int> ans(n, INT_MAX);
        vector<vector<vector<int>>> visit(n, vector(n, vector<int>(2)));
        ans[0] = 0;
        q.emplace(0, 0, 0);
        q.emplace(0, 1, 0);
        visit[0][0][0] = 1;
        visit[0][0][1] = 1;
        while (!q.empty()) {
            auto [x, color, step] = q.front(); q.pop();
            ans[x] = min(ans[x], step);
            for (auto&& g : color ? grid_red[x] : grid_blue[x]) {
                if (visit[x][g][!color]) continue;
                visit[x][g][!color] = 1;
                q.emplace(g, !color, step + 1);
            }

        }

        for (auto&& x : ans) {
            if (x == INT_MAX) x = -1;
        }
        return ans;
    }
};
```