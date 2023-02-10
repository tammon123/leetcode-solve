# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为最小速度越小，总共需要的时间越大，满足单调性。我们可以在$[1,10^7]$二分查找最小速度。总时间$total<=hour$，$total$还可以更大，那么最小速度还能小，收敛上界，否则收敛下界。

2. 因为$hour$有两位精度，我们可以转成整数对比，两边都乘以$100$。有题目知道，前面都是向上取整，最后是求精度，那么我们把前面时间都向上取整，最后一个乘以$target$，然后加上$dist[n-1]$，有: $(\sum\limits_{i=0}\limits^{n-2} (\lfloor \frac{dist[i]-1}{target} \rfloor+1)*target+dist[n-1])*100<=hour*100$

### 复杂度
- 时间复杂度:
  > $O(nlogk)$，$n$为数组$dist$的大小，二分需要$O(logk)$，$k$这里为$10^7$，每次二分都需要遍历数组$O(n)$，所以最终为$O(nlogk)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minSpeedOnTime(vector<int>& dist, double hour) {
        int left = 1, right = (int)1e7, n = dist.size(), ans = -1;
        long long llhour = llround(hour * 100);
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            long long total = 0ll;
            for (int i = 0;i < n - 1;++i) {
                total += (dist[i] - 1) / mid + 1;
            }
            total *= mid;
            total += dist[n - 1];
            if (total * 100 <= llhour * mid) {
                ans = mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return ans;
    }
};
```