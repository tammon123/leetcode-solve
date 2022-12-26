# 图（出入度）
## 思路
1. 有向图的出入度知识
2. 法官不相信任何人，说明出度为$0$；每个人都相信法官，说明入度等于$n-1$
## 复杂度
- 时间复杂度:
  > $O(n)$，n表示人数
- 空间复杂度:
  > $O(n)$，每个人的出入度数据

## Code
```C++ []
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<int> inDegree(n + 1), outDegree(n + 1);
        for (auto&& x : trust) {
            ++outDegree[x[0]];
            ++inDegree[x[1]];
        }

        for (int i = 1;i <= n;++i) {
            if (inDegree[i] == n - 1 && outDegree[i] == 0) return i;
        }
        return -1;
    }
};
```