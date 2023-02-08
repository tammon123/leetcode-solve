# 二分查找
## 方法一(二分查找)：
### 思路
1. 假如当前珂珂的吃香蕉速度是$k$（单位：根/小时），那么每堆香蕉需要的时间为$\lceil \frac{piles[i]}{k} \rceil$，那么所需要的总时间为$total=\sum\limits_{i=1}\limits^{n-1} \lceil \frac{piles[i]}{k} \rceil$，如果$total<=h$，说明$k$值还应该更小才会让$total$更大，查找$k$值过程满足单调性，那么可以用二分方法在$[1,max(piles)]$中查找$k$值。如果$total<=h$，收敛上界，否则收敛下界。

2. 因为都是正整数，所以$\lceil \frac{piles[i]}{k}  \rceil$可以转为$\lfloor \frac {piles[i]+k-1}{k} \rfloor$

### 复杂度
- 时间复杂度:
  > $O(nlogm)$，$n$为数组$piles$大小，$m$为数组最大值，找最大值需要$O(n)$，二分需要$O(logm)$，每次二分需要遍历数组$O(n)$，所以最终为$O(nlogm)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int right = *max_element(piles.begin(), piles.end());
        int left = 1, n = piles.size(), ans = 0;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            long long total = 0;
            for (auto&& x : piles) total += (x + mid - 1) / mid;
            if (total <= h) {
                ans = mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return ans;
    }
};
```