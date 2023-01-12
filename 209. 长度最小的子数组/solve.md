# 前缀和+二分查找/滑动窗口
## 方法一(前缀和+二分查找)：
### 思路
1. 要求和为$target$的最小长度的子数组，那么相当于求最小长度的区间和，我们可以先求出前缀和。$i$从$0$开始，有：
    - $prefixsum[i+1]=prefixsum[i]+nums[i]$
2. 那么区间和满足：$prefixsum[i]-prefixsum[j]>=target$，即我们要找小的下标$j$满足$prefixsum[j]>prefixsum[i]-target$，且$prefixsum[i]>=target$，那么可以用二分查找```upper_bound```查找最小的下标$j$，区间和的长度为$i-j+1$

### 复杂度
- 时间复杂度:
  > $O(n+nlogn)$，n为数组nums的大小，二分查找需要$O(nlogn)$时间
- 空间复杂度:
  > $O(n)$，存储前缀和

### Code
```C++ []
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        vector<int> prefixsum(n + 1);
        for (int i = 0;i < n;++i) {
            prefixsum[i + 1] = prefixsum[i] + nums[i];
        }

        int ans = INT_MAX;
        for (int i = 0;i <= n;++i) {
            if (prefixsum[i] >= target) {
                int j = upper_bound(prefixsum.begin(), prefixsum.begin() + i, prefixsum[i] - target) - prefixsum.begin();
                ans = min(ans, i - j + 1);
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```
## 方法二(滑动窗口)：
### 思路
1. 假设以$i$为右端点，$j$为左端点的子数组的和大于等于$target$，那么子数组个数为$i-j+1$，我们可以从左往右遍历把和算出来，然后$j$从$0$开始遍历，直到:$\sum_{l=j}^{i}nums[l]<target$，那么以$j$为左端点的左边数只会让和变大，所以当$i+1$时，和变大，我们只需要从刚才的$j$位继续往右遍历就行，直到满足新的$\sum_{l=j}^{i}nums[l]<target$

### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组nums的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size(), sum = 0, ans = INT_MAX, j = 0;
        for (int i = 0;i < n;++i) {
            sum += nums[i];
            while (sum >= target) {
                ans = min(ans, i - j + 1);
                sum -= nums[j];
                ++j;
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```