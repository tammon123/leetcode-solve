# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为原数组是升序且数字均不相等，假如旋转后不是原数组，那么以原结点结尾的最大值的左边单调递增，右边也单调递增，那么最小值再右边，如果旋转后为原数组，最小值再左边
2. 假如$nums[mid]>=nums[0]\&\&nums[mid]>nums[n-1]$，中心在第一种情况的左边，最小值在右边，收敛左边界；如果是原数组，不满足这个条件，收敛右边界，最小值在左边的，符合要求
3. 最终收敛于左边界
### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为数组$nums$的大小，二分时间复杂度$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size(), l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[0] <= nums[mid] && nums[n-1] < nums[mid]) l = mid + 1;
            else r = mid - 1;
        }
        return nums[l];
    }
};
```