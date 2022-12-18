# 分类讨论
## 思路
1. 最多添加$2$条边使所有所有点度数为偶数，那么奇数点的个数只有为$0，2，4$的时候才有可能再添加最多$2$条边后度数都为偶
- 当奇数点为$0$时，不用添加任何边，必定为$true$
- 当奇数点为$2$时：两点之间没有边，返回$true$；当两点之间有边时候，我们可以从$1$遍历到$n$点，只要找到没有连接着两点的边，返回$true$，否则返回$false$
- 当奇数点为$4$时：设$4$个点分别为$w$、$x$、$y$、$z$
  - $w$与$x$不存在边且$y$与$z$不存在边 或者
  - $w$与$y$不存在边且$x$与$z$不存在边 或者
  - $w$与$z$不存在边且$x$与$y$不存在边
  - 只有以上三种情况才返回$true$
## 复杂度
- 时间复杂度:
  > $O(m+n)$，m为edges的大小，n为节点数
- 空间复杂度:
  > $O(m+n)$

## Code
```C++ []
class Solution {
public:
    bool isPossible(int n, vector<vector<int>>& edges) {
        vector<unordered_set<int>> grid(n + 1);
        vector<int> odd;
        for (auto&& e : edges) {
            grid[e[0]].emplace(e[1]);
            grid[e[1]].emplace(e[0]);
        }
        for (int i = 1;i <= n;++i) {
            if (grid[i].size() % 2) odd.emplace_back(i);
        }
        int size = odd.size();
        if (size == 0) return true;
        else if (size == 2) {
            int x = odd[0], y = odd[1];
            if (!grid[x].count(y) && !grid[y].count(x)) return true;
            for (int i = 1;i <= n;++i) {
                if (i != x && i != y && !grid[i].count(x) && !grid[i].count(y)) return true;
            }
        }
        else if (size == 4) {
            int w = odd[0], x = odd[1], y = odd[2], z = odd[3];
            if ((!grid[w].count(x) && !grid[y].count(z)) ||
            (!grid[w].count(z) && !grid[x].count(y)) ||
            (!grid[w].count(y) && !grid[x].count(z))) return true;
        }
        return false;
    }
};
```