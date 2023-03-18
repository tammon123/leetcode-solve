# 前缀和+哈希表
## 方法一(前缀和+哈希表)：
### 思路
1. 因为选择两个$k$位为$1$的数减去$2^k$次，相当于异或操作。最终要求子数组都为$0$，即子数组异或后为$0$。

2. 求子数组数目，那么可以考虑转化为数组区间异或为$0$，那么想到前缀和的方式。那就转化为以当前数为结尾，找到之前等于当前数的前缀和的数量。可用哈希表来计数。如例子:
```
4,3,1,2,4
0,4,7,6,4,0
当到$2,4$位会找到出现过的次数为：1+1=2
```

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$长度
- 空间复杂度:
  > $O(n)$，哈希表需要$O(n)$大小来计数

### Code
```C++ []
class Solution {
public:
    long long beautifulSubarrays(vector<int>& nums) {
        unordered_map<long long, long long> um;
        um[0] = 1;
        long long sum = 0ll, ans = 0ll;
        for (auto&& x : nums) {
            sum ^= x;
            ans += um[sum]++;
        }
        return ans;
    }
};
```