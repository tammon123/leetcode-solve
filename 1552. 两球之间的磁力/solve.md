# 二分查找
## 方法一(二分查找)：
### 思路
1. 首先我们可以考虑，假如只有两个篮子两个球，那么自然各放一个。如果有三个球的位置满足$x<y<z$，那么大磁力肯定为$min(y-x,z-y)$。以上都是考虑球固定的情况下，那么也就是说两球之间的距离是固定了。假如所有球固定后，最小的间距为$x$，那我们只需要排序后算出有间距$>=x$的的数量$cnt$。如果$cnt>=m$，说明以$x$作为最小间距是合理的。

2. $x$越大，$cnt$数量越小，满足单调性，可用二分查找$x$。如果$cnt>=m$，说明$cnt$还可以更小，那么$x$应该更大，收敛下边界，否则收敛上界。

3. 如何计算间距数量呢，我们可以用贪心思想，最小左边界肯定是从$pre=position[0]$开始，每次有$position[i]-pre>=x$，那么就更新$pre$，并且数量加一
### 复杂度
- 时间复杂度:
  > $O(nlognl)$，$n$为数组$position$数量，$l$为最大间距，排序需要$O(nlogn)$，二分需要$O(logl)$，每次二分需要遍历$position$数组$O(n)$，所以最终为$O(nlogn+nlogl)$
- 空间复杂度:
  > $O(logn)$，排序需要的栈空间

### Code
```C++ []
class Solution {
public:
    int maxDistance(vector<int>& position, int m) {
        sort(position.begin(), position.end());
        int n = position.size(), left = 1, right = position[n - 1] - position[0], ans = 0;

        auto check = [&](int x)->bool {
            int cnt = 1;
            int pre = position[0];
            for (int i = 1;i < n;++i) {
                if (position[i] - pre >= x) {
                    pre = position[i];
                    ++cnt;
                }
            }
            return cnt >= m;
        };

        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (check(mid)) {
                ans = mid;
                left = mid + 1;
            }
            else right = mid - 1;
        }
        return ans;
    }
};
```