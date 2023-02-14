# 二分查找
## 方法一(二分查找)：
### 思路
1. 假设当前点为$i$，由于数组是从小到大排序好的，$i$越小$citations[i]$也越小，满足单调性，可用二分查找。那么点$i$以后包含点$i$有$n-i$篇论文，那么至少引用$n-i$次，即:$citations[i]>=n-i$。要$n-i$大，$i$应更小，收敛右边界，否则收敛左边界。

### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为数组$citations$的大小，二分需要$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size(), left = 0, right = n - 1, ans = 0;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (citations[mid] >= n - mid) {
                ans = n - mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return ans;
    }
};
```