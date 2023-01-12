# 哈希表
## 方法一(哈希表)：
### 思路
1. 哈希表计数模拟

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$num$的长度
- 空间复杂度:
  > $O(k)$，哈希表最大存储$10$个数

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