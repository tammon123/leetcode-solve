# 二分查找
## 方法一(二分查找)：
### 思路
1. 假设当前遍历到$nums[i]$有$i>1\&\&i<n-1\&\&nums[i-1]<nums[i]<nums[i+1]$说明找到峰值了，返回$i$
2. 如果$i<n-1\&\&nums[i]<nums[i+1]$，说明峰值在右边，收敛左边界，反之收敛右边界
3. 最终收敛为左边界
### 复杂度
- 时间复杂度:
  > $O(logn)$，n为数组nums的大小，二分时间复杂度logn
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size(), l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (mid > 1 && mid < n - 1 && nums[mid] > nums[mid - 1] && nums[mid] > nums[mid + 1]) return mid;
            else if (mid < n - 1 && nums[mid] < nums[mid + 1]) l = mid + 1;
            else r = mid - 1;
        }
        return l;
    }
};
```