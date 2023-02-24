# 二分查找
## 思路
1. 假设最小值开销为$y$，那么$nums$中的每个值转为$y$需要$\lfloor \frac{num[i]-1}{y}\rfloor$次操作，例如：
```
nums = [2,4,8,2], maxOperations = 4
最小开销为2，那么nums[0]=2不用变
nums[1]=4，需要(4-1)/2=1次
nums[2]=8，需要(8-1)/2=3次
nums[3]=2不用变
总共需要4次
```
2. 那么我们只需要遍历查找$1$到$max(nums)$中的$y$值，然后把总开销与$maxOperations$进行对比就行，过程可用二分查找进行
## 复杂度
- 时间复杂度:
  > $O(nlogk)$，$n$为$nums$的大小，$k$为$nums$最大值
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    int minimumSize(vector<int>& nums, int maxOperations) {
        int n = nums.size(), l = 1, r = *max_element(nums.begin(), nums.end()), ans = 0;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            int opt = 0;
            for (auto&& num : nums) {
                opt += (num - 1) / mid;
            }
            if (opt <= maxOperations) ans = mid, r = mid - 1;
            else l = mid + 1;
        }
        return ans;
    }
};
```