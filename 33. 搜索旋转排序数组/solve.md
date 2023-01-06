# 二分查找
## 方法一(二分查找)：
### 思路
1. 旋转后的数组一定是以某以位置$i$，$[0,i]$单调递增，$[i+1,n-1]$单调递增，分割成了两部分递增数组
2. 那么我们可以二分后，判断当前$mid$点处于哪一部分：
   - 如果$nums[0]<=nums[mid]$，处于$[0,i]$部分，然后再这部分判断是否$nums[0]<=target<nums[mid]$，满足收敛上界，不满足收敛下界
   - 如果$nums[0]<nums[mid]$，处于$[i+1,n-1]$部分，这部分以$mid$为起点判断，$nums[mid]<=target<nums[n-1]$，满足收敛下界，不满足收敛上界
### 复杂度
- 时间复杂度:
  > $O(logn)$，n为数组nums的大小，二分时间复杂度logn
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size(), l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] == target) return mid;
            else if (nums[0] <= nums[mid]) {
                if (nums[0] <= target && target < nums[mid]) r = mid - 1;
                else l = mid + 1;
            }
            else {
                if (nums[mid] <= target && target <= nums[n - 1]) l = mid + 1;
                else r = mid - 1;
            }
        }
        return -1;
    }
};
```