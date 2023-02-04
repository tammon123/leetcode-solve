# 排序+最小堆
## 方法一(排序+最小堆)：
### 思路
1. 我们要求$k$位子序列和与它们的最小值的乘积，需要考虑最小值越大越好，那我们可以将$nums2$从大到小的排序，第$k-1$位肯定是最大的最小值(从$0$开始考虑)，那它们的和不一定是最大值。假设按照$nums2$从大到小的原下标数组为$ids$，前$k-1$位最大分数为$nums2[ids[i]]*\sum _0^{k-1}nums1[ids[i]]$。越往右，$nums2$作为最小值越小，整个乘积要想变大，那么和应该变大。即当$k<=j<n$时，$nums1[ids[j]]>\min\limits_{k} (nums1[ids[i]])$。

2. $k$个数中获取最小值，我们可以考虑用最小堆维护。当$nums1[j]$大于堆顶元素，我们把和加上增量，计算最大分数，弹出堆顶元素，并插入新值。

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums1$与$nums2$的大小，堆内操作需要$O(logn)$
- 空间复杂度:
  > $O(n)$，堆最多存储$n$个元素

### Code
```C++ []
class Solution {
public:
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {
        int n = nums1.size();
        vector<int> ids(n);
        iota(ids.begin(), ids.end(), 0);
        sort(ids.begin(), ids.end(), [&](int i, int j) {return nums2[i] > nums2[j];});

        long long sum = 0, ans = 0;
        priority_queue<int, vector<int>, greater<int>> pq;
        for (int i = 0;i < k;++i) {
            sum += nums1[ids[i]];
            pq.emplace(nums1[ids[i]]);
        }
        ans = sum * nums2[ids[k - 1]];

        for (int i = k;i < n;++i) {
            if (nums1[ids[i]] > pq.top()) {
                sum += nums1[ids[i]] - pq.top();
                ans = max(ans, sum * nums2[ids[i]]);
                pq.pop();
                pq.emplace(nums1[ids[i]]);
            }
        }
        return ans;
    }
};
```
