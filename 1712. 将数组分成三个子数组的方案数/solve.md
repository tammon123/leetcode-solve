# 前缀和+二分查找
## 方法一(前缀和+二分查找)：
### 思路
1. 由题意知道，分成了三段区间和，我们可以先求出前缀和$prefixSum$，那么我们需要找到两个点$[x,y]$，有:
   
   - $prefixSum[y]-prefixSum[x]>=prefixSum[x]$
   
   - $prefixSum[n]-prefixSum[y]>=prefixSum[y]-prefixSum[x]$。
   
   联合两条件有:$prefixSum[n]>=2*prefixSum[y]-prefixSum[x]>=3*prefixSum[x]$

2. 由于前缀和是递增的，我们固定左端点$x$后，二分查找满足上面两个条件的$y$，那么在两个式子找到的$y$值范围的任何位置都能满足，最终就是两个$y$值的位置差值$r-l+1$。注意上面联合条件式子，我们可以先判断$prefixSum[n]<3*prefixSum[x]$直接退出循环。

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$表示数组$nums$的大小，需要两次二分$O(logn)$
- 空间复杂度:
  > $O(n)$，前缀和数组需要的大小

### Code
```C++ []
class Solution {
public:
    static const int MOD = 1e9 + 7;
    int waysToSplit(vector<int>& nums) {
        int n = nums.size();
        vector<long long> prefix_sum(n + 1);
        for (int i = 0;i < n;++i) {
            prefix_sum[i + 1] = prefix_sum[i] + nums[i];
        }
        long long ans = 0ll;
        for (int i = 1;i < n - 1;++i) {
            if (prefix_sum[i] * 3 > prefix_sum[n]) break;
            int l = i + 1, r = n - 1;
            while (l <= r) {
                int mid = l + ((r - l) >> 1);
                if (prefix_sum[n] - prefix_sum[mid] >= prefix_sum[mid] - prefix_sum[i]) {
                    l = mid + 1;
                }
                else r = mid - 1;
            }

            int ll = i + 1, rr = n - 1;
            while (ll <= rr) {
                int mid = ll + ((rr - ll) >> 1);
                if (prefix_sum[mid] - prefix_sum[i] >= prefix_sum[i]) {
                    rr = mid - 1;
                }
                else ll = mid + 1;
            }

            ans = (ans + r - ll + 1) % MOD;
        }
        return ans;
    }
};
```