# 二分查找
## 方法一(二分查找)：
### 思路
1. 两次二分，注意判断边界条件，这里取$left<=right$作为循环条件，查找第一个元素就用$nums[mid]<target$来收敛下界，反之收敛上界，最终左值为第一个元素位置
2. 查找最后一个元素，用$nums[mid]<=target$收敛下界，反之收敛上界，最终右值为最后一个元素
3. 如果最终右值大于等于左值，说明$target$是存在数组中的，返回左右值，如果右值小于左值，说明$target$不存在数组中，返回$-1$
### 复杂度
- 时间复杂度:
  > $O(logn)$，n为数组nums的大小，二分时间复杂度logn
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size(), l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] < target) l = mid + 1;
            else r = mid - 1;
        }
        int begin = l;
        l = 0, r = n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (nums[mid] <= target) l = mid + 1;
            else r = mid - 1;
        }
        int end = r;
        vector<int> ans{ -1,-1 };
        if (end >= begin) ans[0] = begin, ans[1] = end;
        return ans;
    }
};
```