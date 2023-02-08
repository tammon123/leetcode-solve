# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为是除法向上取整求和，除数越大总和越小，具有单调性。我们可以用二分查找这个除数，然后总和对比阈值。总和小于等于阈值，说明总和还可以更大，除数需要更小，那么收敛上界，否则收敛下界。

2. 除数只会在数组最小值与最大值中产生。
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$大小，求出最大值需要$O(n)$，二分需要$O(logm)$，$m$上下限之差
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int smallestDivisor(vector<int>& nums, int threshold) {
        int left = 1, right = *max_element(nums.begin(), nums.end()), n = nums.size(), ans = 0;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            int sum = 0;
            for (auto&& x : nums) {
                sum += (x + mid - 1) / mid;
            }
            if (sum <= threshold) {
                ans = mid;
                right = mid - 1;
            }
            else left = mid + 1;
        }
        return ans;
    }
};
```