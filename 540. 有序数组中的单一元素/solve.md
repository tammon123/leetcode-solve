# 二分查找
## 方法一(二分查找)：
### 思路
1. 假设当前单一元素为$x$，那么它与左右元素都不同。正常成对的数，如果当前元素下标是奇数，那么它一定与它前一个元素成对，即：$nums[i]=nums[i-1]$，当前元素下标是偶数，那么它一定与后一个元素成对，即:$nums[i]=nums[i+1]$。正常成对，说明单一元素$x$应该再它们右边，由于数组是有序的，我们可以二分查找$x$，如果满足上面条件说明$x$再右边，收敛左边界，否则收敛右边界。

2. 当前下标是偶数时，$i+1=i\oplus1$。当前下标是奇数时，$i-1=i\oplus1$。可以统一为$nums[i]=nums[i\oplus1]$

### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为数组$nums$的大小，二分需要$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int n = nums.size(), left = 0, right = n - 1;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (nums[mid] == nums[mid ^ 1]) {
                left = mid + 1;
            }
            else right = mid;
        }
        return nums[left];
    }
};
```