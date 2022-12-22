# 动态规划/排序+贪心
## 方法一(动态规划，题目数量超时)：
### 思路
1. 我们可以对区间按照$[start_i]$从小到大排序，那么问题就转化为最大上升子序列问题，设$dp_i$为以区间$i$为最后一个区间的最大上升子序列数量，状态转移有：
   $dp[i]=\max\limits_{j<i \& r_j<=l_i}\{dp[j]\}+1$

我们选出$j<i$中最大的上升子序列，并且满足不重叠要求。因为已经按照左端排序了，那么只需要满足右端$r_j<=l_i$条件进行状态转移就行，如果$max(dp[j])=0$，那么$dp[i]=1$

### 复杂度
- 时间复杂度:
  > $O(nlogn+n^2)$，n为区间数量，nlogn为排序时间复杂度
- 空间复杂度:
  > $O(n+logn)$， logn为排序所需要的空间复杂度

### Code
```C++ []
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        int n = intervals.size(), ans = 1;
        vector<int> dp(n, 1);
        for (int i = 1;i < n;++i) {
            for (int j = 0;j < i;++j) {
                if (intervals[j][1] <= intervals[i][0]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
            ans = max(ans, dp[i]);
        }
        return n - ans;
    }
};
```
## 方法二(排序+贪心):
### 思路
1. 题目其实是求最大上升子序列的过程，最终为$n-数量$
2. 我们可以对$end_i$进行升序排序，那么只要下一位$start_{i+1}>=end_i$说明可以形成最大上升子序列，数量加一，并把$end_i=end_{i+1}$
### 复杂度
- 时间复杂度:
  > $O(nlogn+n)$，n为区间数量，nlogn为排序时间复杂度
- 空间复杂度:
  > $O(logn)$，排序所需要的空间复杂度

### Code
```C++ []
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [&](const vector<int>& v1, const vector<int>& v2) {
            return v1[1] < v2[1];
        });
        int ans = 1, end = intervals[0][1], n = intervals.size();
        for (int i = 1;i < n;++i) {
            if (intervals[i][0] >= end) {
                ++ans;
                end = intervals[i][1];
            }
        }
        return n - ans;
    }
};
```