# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 我们观察可以得到，如果所有的连通路径只要有交点（除了```(0,0)```于```(m-1,n-1)```两点不能翻转），我们翻转交点就能使矩阵不连通。那么，只要有大于等于两条不相交的路径，我们一定不能连通。

2. 两条不相交的路径只需要通过两次深搜，一次深搜后把遍历的点$1$全部转化为$0$后，再次深搜如果还有连通路径，说明找到两条连通路径了，返回$false$。否则返回$true$


### 复杂度
- 时间复杂度:
  > $O(mn)$，$m，n$分别为矩阵的行与列，最多需要搜索完整个矩阵
- 空间复杂度:
  > $O(m+n)$，栈空间需要大小

### Code
```C++ []
class Solution {
public:
    bool isPossibleToCutPath(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        auto dfs = [&](int x, int y, auto&& dfs)->bool {
            if (x >= m || y >= n) return false;
            if (!grid[x][y]) return false;
            if (x == m - 1 && y == n - 1) return true;

            if (x != 0 || y != 0) grid[x][y] = 0;
            return dfs(x + 1, y, dfs) || dfs(x, y + 1, dfs);
        };
        return !dfs(0, 0, dfs) || !dfs(0, 0, dfs);
    }
};
```