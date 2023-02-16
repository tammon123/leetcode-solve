# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为数组中可以存在重复元素，那么我们如果进行二分无法判断是在那一部分，只能让$left++,--right$。

2. 
   - 如果$nums[left]<=nums[mid]$，说明区间$[left,mid]$有序，如果有$nums[left]<=target\&\&target<nums[mid]$，收敛右边界，否则需要收敛左边界。
   - 如果$nums[left]>nums[mid]$，说明区间$[mid,right]$有序，如果有$nums[mid]<target\&\&target<=nums[right]$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小，最坏情况下所有数都相等不等于$target$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size(), left = 0, right = n - 1;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == target) return true;
            if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
                ++left;
                --right;
            }
            else if (nums[left] <= nums[mid]) {
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1;
                }
                else left = mid + 1;
            }
            else {
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1;
                }
                else right = mid - 1;
            }
        }
        return false;
    }
};
```