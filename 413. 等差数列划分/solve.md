# 差分+计数
## 方法一(差分+计数)：
### 思路
1. 等差数列公差$d$都是相等的，假设$nums[i]-nums[i-1]=d$，等差子数组个数为$cnt$。新增一个数据还满足等差数组，即$nums[i+1]-nums[i]=d$，那么等差子数组个数会增加$cnt+1$个，最终等差子数组个数为$cnt+cnt+1$个。如新增数据公差与上一个不公差相等，那么公差更新，计数$cnt$清零。
2. 所以我们只需要维护一个公差与计数，就能计算出最终所有子数组个数

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(1)$
  
### Code
```C++ []
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return 0;
        int cnt = 0, diff = nums[1] - nums[0], ans = 0;
        for (int i = 2;i < n;++i) {
            int temp = nums[i] - nums[i - 1];
            if (temp == diff) ++cnt;
            else cnt = 0, diff = temp;
            ans += cnt;
        }
        return ans;
    }
};
```