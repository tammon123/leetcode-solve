# 线性扫描
## 方法一(线性扫描)：
### 思路
1. 假设有$nums[i]<nums[j]$，那么区间$[nums[i]+k,nums[j]-k]$一定是$[nums[i]-k,nums[j]+k]$的子集，所以只需要把数组排序，每次扫描四个点```nums[0]+k,nums[i]+k,nums[i+1]-k,nums[n-1]-k```区间最小差值就行

### 复杂度
- 时间复杂度:
  > $O(n+nlogn)$，n为数组nums的大小，nlogn为排序复杂度
- 空间复杂度:
  > $O(1)$，存储前缀和

### Code
```C++ []
class Solution {
public:
    int smallestRangeII(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), firstmin = nums[0] + k, endmax = nums[n - 1] - k, ans = nums[n - 1] - nums[0];
        for (int i = 0;i < n - 1;++i) {
            int a = nums[i], b = nums[i + 1];
            ans = min(ans, max(endmax, a + k) - min(firstmin, b - k));
        }
        return ans;
    }
};
```