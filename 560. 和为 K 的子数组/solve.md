# 前缀和+哈希表
## 思路
1. 要求$nums$中和能组成$k$的连续子数组个数，我们可以以$i$为右边界，遍历$0<=j<=i$的子数组和能组成，但这样时间复杂度很大
2. 我们可以转化一下，预处理前缀和$prefix$，那么满足：$prefix[i]-prefix[j-1]=k$的都能成立。把式子变化一下，那么有$prefix[j-1]=prefix[i]-k$，我们可以用哈希表存储$prefix[j-1]$的数量，就不用遍历$0<=j<=i$。又因为$prefix[i]$只与前一位有关，我们可以用一个变量$pre$滚动压缩前缀和数组。
3. 注：$prefix[i]-k=0$表示自身就能组成$k$，那么哈希表$[0]=1$

## 复杂度
- 时间复杂度:
  > $O(mlogn)$，m，n分别为matrix的行跟列
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int n = nums.size(), ans = 0, pre = 0;
        unordered_map<int, int> um;
        um[0] = 1;
        for (int i = 0;i < n;++i) {
            pre += nums[i];
            if (um.count(pre - k)) ans += um[pre - k];
            ++um[pre];
        }
        return ans;
    }
};
```