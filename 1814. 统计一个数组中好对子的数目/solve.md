# 哈希表
## 方法一(哈希表)：
### 思路
1. 可以先预处理所有反转后的数字
2. $nums[i] + rev(nums[j]) == nums[j] + rev(nums[i])<=>nums[i] - rev(nums[i]) == nums[j] - rev(nums[j])$，那么我们在每次遍历到$i$时加上哈希表中$nums[i]-rev(nums[i])$的数量，并让哈希表$nums[i]-rev(nums[i])$加一
   

### 复杂度
- 时间复杂度:
  > $O(n*logk)$，$n$为数组$nums$的大小，$O(logk)$为反转数字的时间复杂度
- 空间复杂度:
  > $O(n)$，预处理数组跟哈希表需要存储数组的个数

### Code
```C++ []
class Solution {
public:
    static const int MOD = 1e9 + 7;

    int countNicePairs(vector<int>& nums) {
        auto rev = [&](int num)->int {
            int t = 0;
            while (num) {
                t = t * 10 + num % 10;
                num /= 10;
            }
            return t;
        };
        int n = nums.size();
        vector<int> rev_nums(n);
        for (int i = 0;i < n;++i) {
            rev_nums[i] = rev(nums[i]);
        }

        unordered_map<int, int> um;
        int ans = 0;
        for (int i = 0;i < n;++i) {
            ans = (long)(ans + um[nums[i] - rev_nums[i]]++) % MOD;
        }

        return ans;
    }
};
```