# 深度优先搜索/广度优先搜索/并查集
## 方法一(深度优先搜索)：
### 思路
1. 深搜遇到$'1'$，答案加一，并让$'1'$变为$'0'$，防止再次访问

### 复杂度
- 时间复杂度:
  > $O(mn)$，m，n分别为矩阵grid的行数跟列数
- 空间复杂度:
  > $O(mn)$，最坏复杂度全部都是1,递归栈深度为mn

### Code
```C++ []
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        auto dfs = [&](int x, int y, auto&& dfs) -> void {
            if (x >= 0 && x < m && y >= 0 && y < n && grid[x][y] == '1') {
                grid[x][y] = '0';
                dfs(x + 1, y, dfs);
                dfs(x - 1, y, dfs);
                dfs(x, y + 1, dfs);
                dfs(x, y - 1, dfs);
            }
        };

        int ans = 0;
        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (grid[i][j] == '1') {
                    ++ans;
                    dfs(i, j, dfs);
                }
            }
        }

        return ans;
    }
};
```
## 方法二(广度优先搜索)：
### 思路
1. 遍历矩阵，为$'1'$，答案加一，并进行广搜，如深搜一样把$'1'$转化为$'0'$

### 复杂度
- 时间复杂度:
  > $O(mn)$，m，n分别为矩阵grid的行数跟列数
- 空间复杂度:
  > $O(min(m,n))$，最坏情况全部为1，那么队列中最多存储min(m,n)个

### Code
```C++ []
class Solution {
public:
    const vector<int> direction = { 0, -1, 0, 1, 0 };
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size(), ans = 0;
        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (grid[i][j] == '1') {
                    ++ans;
                    queue<pair<int, int>> q;
                    q.emplace(i, j);
                    while (!q.empty()) {
                        auto [x, y] = q.front(); q.pop();
                        for (int i = 0;i < 4;++i) {
                            int cx = x + direction[i], cy = y + direction[i + 1];
                            if (cx < 0 || cy < 0 || cx >= m || cy >= n) continue;
                            if (grid[cx][cy] == '0') continue;
                            grid[cx][cy] = '0';
                            q.emplace(cx, cy);
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

## 方法三(并查集)：
### 思路
1. 使用并查集连通，遍历矩阵，如果为$'1'$，连通上下左右的$'1'$，并将当前点设为$'0'$
2. 最后并查集的连通分量的数目就是岛屿数

### 复杂度
- 时间复杂度:
  > $O(mn*\alpha(mn) )$，m，n分别为矩阵grid的行数跟列数，单次操作并查集时间复杂度为反阿克曼函数$\alpha(mn)$
- 空间复杂度:
  > $O(mn)$，并查集所需空间

### Code
```C++ []
class UnionFind {
public:
    UnionFind(vector<vector<char>>& grid) {
        m_setCount = 0;
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0;j < n;++j) {
                if (grid[i][j] == '1') {
                    m_parent.emplace_back(i * n + j);
                    ++m_setCount;
                }
                else m_parent.emplace_back(-1);
                m_rank.emplace_back(0);
            }

        }
    }

    void merge(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) return;

        if (m_rank[pRoot] < m_rank[qRoot]) m_parent[pRoot] = qRoot;
        else if (m_rank[qRoot] < m_rank[pRoot]) m_parent[qRoot] = pRoot;
        else {
            m_parent[pRoot] = qRoot;
            m_rank[qRoot] += 1;
        }
        --m_setCount;
    }

    int find(int p) {
        if (p != m_parent[p])
            m_parent[p] = find(m_parent[p]);
        return m_parent[p];
    }

    bool is_connect(int p, int q) {
        return find(p) == find(q);
    }

    int diff_union_size() {
        return m_setCount;
    }

private:
    vector<int> m_parent;
    vector<int> m_rank;
    int m_setCount;
};

class Solution {
public:
    const vector<int> direction = { 0, -1, 0, 1, 0 };
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size(), ans = 0;
        UnionFind uf(grid);
        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (grid[i][j] == '1') {
                    grid[i][j] = '0';
                    for (int k = 0;k < 4;++k) {
                        int cx = i + direction[k], cy = j + direction[k + 1];
                        if (cx < 0 || cy < 0 || cx >= m || cy >= n) continue;
                        if (grid[cx][cy] == '0') continue;
                        uf.merge(i * n + j, cx * n + cy);
                    }
                }
            }
        }

        return uf.diff_union_size();
    }
};
```