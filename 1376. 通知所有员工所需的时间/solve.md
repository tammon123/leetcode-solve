# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 通过遍历直属负责人表，把除了总负责人$-1$外的所有负责人所需管理的人存入数组中，即用二维数组$grid[manager[i]]=i$
2. 然后从总负责人开始深搜$grid$表，同一层的求出最大所需时间，最大时间为本人以及所管理的员工所需要的总负责时间，同层为同时通知，所以求出最大值，自上往下递归
### 复杂度
- 时间复杂度:
  > $O(n)$，n为员工数
- 空间复杂度:
  > $O(n)$，数组存储所有员工，递归栈不会大于n，所以最终为n

### Code
```C++ []
class Solution {
public:
    int numOfMinutes(int n, int headID, vector<int>& manager, vector<int>& informTime) {
        vector<vector<int>> grid(n);
        for (int i = 0;i < n;++i) {
            if (manager[i] == -1) continue;
            grid[manager[i]].emplace_back(i);
        }

        auto dfs = [&](int cur, auto&& dfs)->int {
            int maxinform = 0;
            for (auto&& g : grid[cur]) {
                maxinform = max(maxinform, informTime[g] + dfs(g, dfs));
            }
            return maxinform;
        };
        return informTime[headID] + dfs(headID, dfs);
    }
};
```