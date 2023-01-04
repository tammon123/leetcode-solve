# 数学（切比雪夫距离）
## 方法一(数学)：
### 思路
1. 切比雪夫距离定义为，两个点之间各坐标数值差的绝对值的最大值
2. 只需遍历数组求出每两个点的切比雪夫距离之和，就是最终答案
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) {
        int ans = 0, n = points.size();
        for (int i = 0;i < n - 1;++i) {
            ans += max(abs(points[i][0] - points[i + 1][0]), abs(points[i][1] - points[i + 1][1]));
        }
        return ans;
    }
};
```