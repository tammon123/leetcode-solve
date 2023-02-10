# 二分查找
## 方法一(二分查找)：
### 思路
1. 假设当前等待的天数为$x$，我们遍历数组，如果连续出现$k$次$>=x$的，说明这$k$次花开了可以做成一束花，最终看在$x$天时候能做成的花束$total$是否大于等于$m$。

2. 由于$x$越大，$total$越大，满足单调性。当$total>=m$时，说明$total$还可以更小，$x$可以更小，收敛上边界，否则收敛下边界。
### 复杂度
- 时间复杂度:
  > $O(nlogl)$，$n$是数组$bloomDay$的大小，$l$为数组中最大值，找出数组中最大值需要$O(n)$，我们需要二分$O(logl)$，每次二分需要遍历数组$O(n)$，最终需要$O(nlogl)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minDays(vector<int>& bloomDay, int m, int k) {
        int left = 1, right = *max_element(bloomDay.begin(), bloomDay.end()), n = bloomDay.size(), ans = 0;
        if ((long)m * k > n) return -1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            int total = 0, cnt = 0;
            for (auto&& day : bloomDay) {
                if (day <= mid) {
                    ++cnt;
                    if (cnt == k) {
                        ++total;
                        cnt = 0;
                    }
                }
                else cnt = 0;
            }

            if (total >= m) {
                ans = mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return ans;
    }
};
```