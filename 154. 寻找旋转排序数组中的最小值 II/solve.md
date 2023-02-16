# 二分查找
## 方法一(二分查找)：
### 思路
1. 由于旋转后，以最小值来划分两部分。

2. 我们二分答案，左端点为$left$，右端点为$right$，中点为$mid$。以此来分类讨论：
   - 如果$nums[mid]=nums[left] \&\& nums[mid]=nums[right]$，我们无法判断最小值在$mid$左边还是右边，我们同时缩减左右范围。
   - 如果$nums[mid]<=nums[right]$，说明在最小值在$mid$点或者之前，需要收敛右边界
   - 其他情况收敛左边界
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
        int n = nums.size(), left = 0, right = n - 1;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
                ++left;
                --right;
            }
            else if (nums[mid] <= nums[right]) right = mid;
            else left = mid + 1;
        }
        return nums[left];
    }
};
```