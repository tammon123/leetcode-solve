# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为数组中总有峰值，那么假设当前遍历到$nums[i]$，如果有$nums[i]>nums[i+1]$，那么说明峰值再当前节点或者之前，收敛右边界，否则收敛左边界。

2. 假设只有一个节点，根据条件，它一定是峰值，我们再处理循环条件时候令$left=0,right=n-1$，那么只需要$left<right$就行。最终返回$left$的位置。
### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为数组$nums$的大小，二分时间复杂度$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size(), left = 0, right = n - 1;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] > nums[mid + 1]) {
                right = mid;
            }
            else left = mid + 1;
        }
        return left;
    }
};
```