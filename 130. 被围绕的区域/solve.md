# 深度优先搜索/广度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 只是把围绕区域的$'O'$变为$'X'$，那么临边与临边相邻的所有$'O'$都不变，那么我们可以先通过深搜把临边以及临边相邻的所有$'O'$都转为其他字符，例如:$'A'$
2. 然后再遍历一遍矩阵，所有$'O'$转为$'X'$，所有$'A'$还原成$'O'$就行

### 复杂度
- 时间复杂度:
  > $O(mn)$，$m$为矩阵的行数，$n$为矩阵的列数
- 空间复杂度:
  > $O(mn)$，栈深度开销，最大为全部都是$'O'$

### Code
```C++ []
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        int m = board.size(), n = board[0].size();
        auto dfs = [&](int x, int y, auto&& dfs)->void {
            if (x < 0 || y < 0 || x >= m || y >= n) return;
            if (board[x][y] != 'O') return;
            board[x][y] = 'A';
            dfs(x + 1, y, dfs);
            dfs(x - 1, y, dfs);
            dfs(x, y + 1, dfs);
            dfs(x, y - 1, dfs);
        };
        for (int j = 0;j < n;++j) {
            if (board[0][j] == 'O') dfs(0, j, dfs);
            if (board[m - 1][j] == 'O') dfs(m - 1, j, dfs);
        }
        for (int i = 1;i < m - 1;++i) {
            if (board[i][0] == 'O') dfs(i, 0, dfs);
            if (board[i][n - 1] == 'O') dfs(i, n - 1, dfs);
        }

        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (board[i][j] == 'A') board[i][j] = 'O';
                else if (board[i][j] == 'O') board[i][j] = 'X';
            }
        }
    }
};
```

## 方法二(广度优先搜索)：
### 思路
1. 同方法一，可以把搜索过程转化为广搜

### 复杂度
- 时间复杂度:
  > $O(mn)$，$m$为矩阵的行数，$n$为矩阵的列数
- 空间复杂度:
  > $O(mn)$，队列开销，最大为全部都是$'O'$

### Code
```C++ []
class Solution {
public:
    const vector<int> direction = { 0, -1, 0, 1, 0 };

    void solve(vector<vector<char>>& board) {
        int m = board.size(), n = board[0].size();
        queue<pair<int, int>> q;
        for (int j = 0;j < n;++j) {
            if (board[0][j] == 'O') q.emplace(0, j);
            if (board[m - 1][j] == 'O') q.emplace(m - 1, j);
        }
        for (int i = 1;i < m - 1;++i) {
            if (board[i][0] == 'O') q.emplace(i, 0);
            if (board[i][n - 1] == 'O') q.emplace(i, n - 1);
        }

        while (!q.empty()) {
            auto [x, y] = q.front(); q.pop();
            board[x][y] = 'A';
            for (int i = 0;i < 4;++i) {
                int cx = x + direction[i], cy = y + direction[i + 1];
                if (cx < 0 || cy < 0 || cx >= m || cy >= n) continue;
                if (board[cx][cy] != 'O') continue;
                q.emplace(cx, cy);
            }
        }

        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (board[i][j] == 'A') board[i][j] = 'O';
                else if (board[i][j] == 'O') board[i][j] = 'X';
            }
        }
    }
};
```