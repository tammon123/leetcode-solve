# 动态规划/贪心+二分查找
## 方法一(动态规划)：
### 思路
1. 假设$dp[i]$表示以$nums[i]$为结束的最长子序列长度，那么他应该满足在$0<=j<i$，有$nums[i]>nums[j]$，以$nums[j]$为结尾的最长自子序列加一，即:$dp[i]=max(dp[j])+1$
2. 边界条件，因为每个数字自身作为最长子序列为$1$，所以初始所有$dp[i]=1$

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为数组$nums$大小，计算每个$dp[i]$时，需要遍历$dp[0...i-1]$状态，所以需要$O(n^2)$复杂度
- 空间复杂度:
  > $O(n)$，存储长度为$n$的$dp$状态数组

### Code
```C++ []
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n, 1);
        for (int i = 1;i < n;++i) {
            for (int j = 0;j < i;++j) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```

## 方法二(贪心+二分查找)：
### 思路
1. 我们想要最长子序列尽可能的长，那么就需要想要每次最长上升序列结尾的数尽可能小
2. 所以我们维护一个长度$dp$，$dp[len]$表示长度为$len$的最长上升序列末尾最小值。过程有：
   - 如果$nums[i]>dp[len]$，说明最长上升序列可以以$nums[i]$结尾，将$len+1$，有$dp[len]=nums[i]$
   - 如果$nums[i]<dp[len]$，那么我们在$1<=j<len$中找到$dp[j-1]<nums[i]<dp[j]$，更新$dp[j]=nums[i]$，由于以上过程可知，$dp$是满足单调递增的，所以我们可以用二分查找加速过程。

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums$大小，计算每个$dp[i]$时，需要二分查找$O(logn)$，所以总时间为$O(nlogn)$
- 空间复杂度:
  > $O(n)$，存储长度为$n$的$dp$状态数组

### Code
```C++ []
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size(), len = 1;
        vector<int> dp(n + 1);
        dp[len] = nums[0];
        for (int i = 1;i < n;++i) {
            if (nums[i] > dp[len]) dp[++len] = nums[i];
            else {
                int l = 1, r = len, pos = 0;
                while (l <= r) {
                    int mid = l + ((r - l) >> 1);
                    if (dp[mid] < nums[i]) {
                        pos = mid;
                        l = mid + 1;
                    }
                    else r = mid - 1;
                }
                dp[pos + 1] = nums[i];
            }
        }
        return len;
    }
};
```