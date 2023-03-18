# 排序+贪心+前缀和
## 方法一(排序+贪心+前缀和)：
### 思路
1. 首先题目已经要求前缀和，所以需要求前缀和。然后分数计数要求前缀和为正整数的最大数量，我们想到会有负数影响前缀和，把数组从小到大排序，采用贪心思想，直到前缀和不再大于$0$，就是最大分数。

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组长度，排序需要$O(nlogn)$时长，还需要$O(n)$遍历数组
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maxScore(vector<int>& nums) {
        sort(nums.begin(), nums.end(), greater<int>());
        long long ans = 0, sum = 0;
        for (auto&& x : nums) {
            sum += x;
            if (sum > 0) ++ans;
            else break;
        }
        return ans;
    }
};
```