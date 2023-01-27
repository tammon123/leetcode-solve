# 动态规划/贪心+前缀和+二分查找
## 方法一(动态规划)：
### 思路
1. 这是[300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)进阶题
1. 假设$dp[i]$表示以$nums[i]$为结束的最长子序列长度，那么他应该满足在$0<=j<i$，有$nums[i]>nums[j]$，以$nums[j]$为结尾的最长自子序列加一，即:$dp[i]=max(dp[j])+1$。而$cnt[i]$表示以$nums[i]$为结束的最长子序列的个数，等于所有满足$dp[j]+1=dp[i]$的$cnt[j]$和。
2. 边界条件，因为每个数字自身作为最长子序列为$1$，所以初始所有$dp[i]=1,cnt[i]=1$

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为数组$nums$大小，计算每个$dp[i]$时，需要遍历$dp[0...i-1]$状态，$cnt[i]$同时更新，所以需要$O(n^2)$复杂度
- 空间复杂度:
  > $O(n)$，存储长度为$n$的$dp,cnt$状态数组

### Code
```C++ []
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size(), maxLen = 1, ans = 1;
        vector<int> dp(n, 1), cnt(n, 1);
        for (int i = 1;i < n;++i) {
            for (int j = 0;j < i;++j) {
                if (nums[i] <= nums[j]) continue;
                if (dp[j] + 1 > dp[i]) {
                    dp[i] = dp[j] + 1;
                    cnt[i] = cnt[j];
                }
                else if (dp[j] + 1 == dp[i]) {
                    cnt[i] += cnt[j];
                }
            }

            if (dp[i] > maxLen) {
                maxLen = dp[i];
                ans = cnt[i];
            }
            else if (dp[i] == maxLen) {
                ans += cnt[i];
            }
        }
        return ans;
    }
};
```

## 方法二(贪心+前缀和+二分查找)：
### 思路
1. 我们想要最长子序列尽可能的长，那么就需要想要每次最长上升序列结尾的数尽可能小
2. 维护二维数组$dp[i]$表示能到达$i+1$长度的最长上升序列的末尾元素值的集合。$dp$每一项的最后元素满足单调递增性，所以我们可以二分查找来插入数据。例如：
```
nums: 1,3,2
dp[0]=1
dp[1]=3,2
```
3. 因为每次都是尽量小的往后插入，那么$dp[i]$非递增，满足单调性。那么我们可以维护一个二维数组$cnt[i][j]$，表示记录了$dp[i][j]$为结尾的最长上升子序列的个数。那么在计算$cnt[i][j]$时，我们只需要考虑$dp[i-1]$与$cnt[i-1]$。我们需要找到所有满足$dp[i-1][k]<dp[i][j]$的$cnt[i-1][k]$加入到$cnt[i][j]$中，那么最终答案就是$cnt[maxLen]$所有元素的和。
4. 因为$dp[i]$满足单调性，我们可以通过二分查找，找到最小的下标$k$，满足$dp[i-1][k] < dp[i][j]$。我们可以优化$cnt[i][j]$，以前缀和的方式存储，开头添加一个$0$，此时$dp[i][j]$对应的最长上升子序列的个数为$cnt[i][-1]-cnt[i][k]$，其中$-1$表示$cnt[i]$最后的元素

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums$大小，对于$nums$中的每个元素，我们需要至少$2$次二分查找$O(logn)$，所以总时间为$O(nlogn)$
- 空间复杂度:
  > $O(n)$，存储长度为$n$的$dp$状态数组与$cnt$数组

### Code
```C++ []
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        vector<vector<int>> dp, cnt;

        auto binarySearch = [&](int n, function<bool(int)> f)->int {
            int l = 0, r = n - 1;
            while (l <= r) {
                int mid = l + ((r - l) >> 1);
                if (f(mid)) {
                    r = mid - 1;
                }
                else l = mid + 1;
            }
            return l;
        };

        for (auto&& num : nums) {
            int i = binarySearch(dp.size(), [&](int i) { return dp[i].back() >= num;});
            int c = 1;

            if (i > 0) {
                int k = binarySearch(dp[i - 1].size(), [&](int k) { return dp[i - 1][k] < num;});
                c = cnt[i - 1].back() - cnt[i - 1][k];
            }

            if (i == dp.size()) {
                dp.push_back({ num });
                cnt.push_back({ 0,c });
            }
            else {
                dp[i].emplace_back(num);
                cnt[i].emplace_back(cnt[i].back() + c);
            }
        }

        return cnt.back().back();
    }
};
```