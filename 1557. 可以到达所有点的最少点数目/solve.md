# 图（入度）
## 思路
1. 有向图的入度知识
2. 点有入度不为$0$，说明有其他点能通向它，那我们只需要找到入度为$0$的点作为起始点，就能把所有点走完
## 复杂度
- 时间复杂度:
  > $O(m+n)$，m为边数，n为点数
- 空间复杂度:
  > $O(n)$，只需要存储点数

## Code
```C++ []
class Solution {
public:
    vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) {
        vector<int> inDegree(n);
        for (auto&& x : edges) {
            ++inDegree[x[1]];
        }
        vector<int> ans;
        for (int i = 0;i < n;++i) {
            if (inDegree[i] == 0) ans.emplace_back(i);
        }
        return ans;
    }
};
```