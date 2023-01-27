# 几何数学
## 思路
1. 按照题目意思，在边上也在圆内，那么坐标点到圆心的距离应该小于等于半径才满足条件

### 复杂度
- 时间复杂度:
  > $O(mn)$，其中$m,n$分别为数组$points,queries$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> countPoints(vector<vector<int>>& points, vector<vector<int>>& queries) {
        int n = queries.size();
        vector<int> ans(n);
        for (int i = 0;i < n;++i) {
            int x = queries[i][0], y = queries[i][1], r = queries[i][2], res = 0;
            for (auto&& pt : points) {
                res += (pow(abs(x - pt[0]), 2) + pow(abs(y - pt[1]), 2)) <= pow(r, 2);
            }
            ans[i] = res;
        }
        return ans;
    }
};
```