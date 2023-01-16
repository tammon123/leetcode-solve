# 递归+回溯
## 方法一(递归+回溯)：
### 思路
1. 我们遍历矩阵中的每一位，如果跟字符串首位相同，那么我们进行迭代，往相邻元素查找。
   - 如果当前迭代元素与字符串不相等，返回$false$
   - 如果找到字符串最后一位相等，说明匹配，我们返回$true$
   - 为了防止访问之前访问过的元素，我们需要数组$visit$标记访问过的元素，遍历相邻元素如果访问过的，就跳过

### 复杂度
- 时间复杂度:
  > $O(mn*3^{target})$，$m,n$为矩阵$board$的行列数，$target$为字符串$word$的长度，除了第一个元素开始可能走$4$个方向，剩余所有位置最多走三个方向，因为字符串$word$长度为$target$，所以最多匹配$3^{target}$次，因为会剪枝，所以最终复杂度会小于$O(mn*3^{target})$
- 空间复杂度:
  > $O(mn)$，$visit$数组会存储$mn$个元素，同时栈的深度为$O(min(target,mn))$

### Code
```C++ []
class Solution {
public:
    const vector<int> direction = { 0, -1, 0, 1, 0 };

    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size(), n = board[0].size(), target = word.size();
        vector<vector<int>> visit(m, vector<int>(n));
        auto dfs = [&](int x, int y, int cur, auto&& dfs)->bool {
            if (board[x][y] != word[cur]) return false;
            if (cur == target - 1) return true;

            visit[x][y] = 1;
            bool t = false;
            for (int i = 0;i < 4;++i) {
                int cx = x + direction[i], cy = y + direction[i + 1];
                if (cx >= 0 && cy >= 0 && cx < m && cy < n && !visit[cx][cy]) {
                    t = dfs(cx, cy, cur + 1, dfs);
                    if (t) return true;
                }
            }
            visit[x][y] = 0;
            return t;
        };
        for (int i = 0;i < m;++i) {
            for (int j = 0;j < n;++j) {
                if (board[i][j] == word[0]) {
                    if (dfs(i, j, 0, dfs)) return true;
                }
            }
        }
        return false;
    }
};
```