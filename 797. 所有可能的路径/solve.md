# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 我们通过深搜从$0$节点开始往数组$graph[0]$开始遍历，一直到$n-1$，过程用临时数组存储数据，然后自下而上的回溯并进行下一条路径搜索

### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，$n$节点数，最坏情况所有节都可以去往编号比它大的点，有$2^n$条路径，所以复杂度为$O(n*2^n)$
- 空间复杂度:
  > $O(n)$，深搜栈空间大小

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<vector<int>> ans;
        vector<int> temp;
        auto dfs = [&](int cur, auto& dfs) ->void {
            if (cur == n - 1) {
                ans.emplace_back(temp); return;
            }
            for (auto&& g : graph[cur]) {
                temp.emplace_back(g);
                dfs(g, dfs);
                temp.pop_back();
            }
        };
        temp.emplace_back(0);
        dfs(0, dfs);
        return ans;
    }
};
```