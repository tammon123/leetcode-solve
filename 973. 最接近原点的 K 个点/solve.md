# 最大堆
## 方法一（最大堆）：
### 思路
1. 最接近原点的$k$个坐标点，可以直接转化为$distance=x^2+y^2$最小的前$k$个
2. 那么我们可以维护一个最大堆，堆存储$[distance,points_{index}]$，当新的$distance$小于堆顶元素，弹出堆顶元素，把新值入堆
3. 最后遍历堆，把对应$points_{index}$的$pints$存入数组中，就是最近的$k$个坐标点集合
### 复杂度
- 时间复杂度:
  > $O(nlogk)$，$n$为$points$的长度，$O(logk)$为堆排序时间复杂度
- 空间复杂度:
  > $O(k)$，大根堆中最多$k$个值

### Code
```C++ []
class Solution {
public:
    using pII = pair<int, int>;
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        priority_queue<pII, vector<pII>> pq;

        int n = points.size();
        for (int i = 0;i < n;++i) {
            int distance = points[i][0] * points[i][0] + points[i][1] * points[i][1];
            if (pq.size() == k) {
                auto [d, index] = pq.top();
                if (distance < d) {
                    pq.pop();
                    pq.emplace(distance, i);
                }
            }
            else pq.emplace(distance, i);
        }
        vector<vector<int>> ans;
        while (!pq.empty()) {
            ans.emplace_back(points[pq.top().second]);
            pq.pop();
        }
        return ans;
    }
};
```
