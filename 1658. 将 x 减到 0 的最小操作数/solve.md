# 滑动窗口
## 方法一(滑动窗口)：
### 思路
1. 求的是左右连续子数组的和，假设左边子数组和为$lsum$，右边子数组和为$rsum$，如果$lsum+rsum=x$，那么随着$lsum$增大，即往右遍历，$rsum$只会变小，当前操作数为左边子数组个数加右边子数组个数，我们不妨假设从左边往右遍历，$left=-1,lsum=0,right=n,rsum=sum(nums)$，左每遍历一个数，找到$lsum+rsum<=x$的右边，求过程的最小操作数
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为$nums$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minOperations(vector<int>& nums, int x) {
        long sum = accumulate(nums.begin(), nums.end(), 0l);
        int n = nums.size(), left = -1, right = 0, lsum = 0, rsum = sum, ans = n + 1;
        for (;left < n;++left) {
            if (left != -1) lsum += nums[left];
            while (right<n && lsum + rsum>x) {
                rsum -= nums[right++];
            }

            if (lsum + rsum == x) ans = min(ans, left + 1 + n - right);
        }
        return ans == n + 1 ? -1 : ans;
    }
};
```