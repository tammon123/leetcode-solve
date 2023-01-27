# 哈希表
## 方法一(哈希表)：
### 思路
1. 一条直线上的斜率相等，那么我们最终遍历点$i$与其他点，统计斜率的次数加一，本身要加

2. 记录斜率问题，因为需要考虑精度与除数为$0$情况，我们可以存分子分母二元组，这样就能避免这问题。
   - $k=\frac{\Delta y}{\Delta x}$，会存在斜率相等，当时分子分母不相等情况，例如:$\frac{1}{2}$与$\frac{2}{4}$，那么我们可以上下都除以最大公约数$gcd(|\Delta y|, |\Delta x|)$，保证最终统计结果正确
   - 还要考虑$\Delta y=0$时，考虑题目不存在重复点，所以我们可以令$\Delta x=1$；同理$\Delta x=0$，令$\Delta y=1$
   - 如果$\Delta y,\Delta x$存在负值，例如:$\frac{-1}{2}=\frac{1}{-2}$，我们可以统一计算，统一只考虑$\Delta y<0$，让$\Delta x,\Delta y$都乘以$-1$

3. 经过以上操作后，我们得到二元组$(\Delta x,\Delta y)$,因为坐标范围在$[-10^4,10^4]$，所以$\frac{\Delta y}{\Delta x}$的最大范围为$[-2*10^4,2*10^4]$，完全可以用$int$类型映射，映射值为$val=\Delta y + \Delta x*20001$

4. 我们可以优化：
   - 在点的数量小于等于$2$的情况下，一条直线能串联所有点，我们直接返回点的数量
   - 我们遍历到点$i$时，我们只需要考虑编号大于$i$的点，因为后面遍历的点的直线已经考虑过$i$点了
   - 当一条直线经历过一半的点，说明它就是经历最多点的直线了
   - 当我们枚举到点$i$时，最多有$n-i$个点共线。如果之前最大共线点数$k>=n-i$，那么可以直接返回，不会找到更大答案了
### 复杂度
- 时间复杂度:
  > $O(n^2logm)$，$n$为点的数量，$m$为横纵坐标差值最大值。最坏情况下，我们需要枚举$n$个点，我们需要计算$O(n)$次最大公约数。最大公约数每次计算需要$O(logm)$时间
- 空间复杂度:
  > $O(n)$，哈希表需要的开销

### Code
```C++ []
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int n = points.size(), ans = 0;
        if (n <= 2) return n;

        for (int i = 0;i < n;++i) {
            if (ans >= n - i || ans > n / 2) break;

            int x1 = points[i][0], y1 = points[i][1];
            unordered_map<int, int> um;
            for (int j = i + 1;j < n;++j) {
                int delta_x = points[j][0] - x1, delta_y = points[j][1] - y1;
                if (delta_x == 0) delta_y = 1;
                else if (delta_y == 0) delta_x = 1;
                else {
                    if (delta_y < 0) delta_x = -delta_x, delta_y = -delta_y;
                    int cgcd = gcd(abs(delta_x), abs(delta_y));
                    delta_x /= cgcd;
                    delta_y /= cgcd;
                }
                ++um[delta_y + delta_x * 20001];
            }

            int maxcnt = 0;
            for (auto&& [_, cnt] : um) {
                maxcnt = max(maxcnt, cnt + 1);
            }
            ans = max(ans, maxcnt);
        }


        return ans;
    }
};
```