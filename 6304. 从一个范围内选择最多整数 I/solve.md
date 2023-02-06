# 哈希表+贪心
## 方法一(哈希表+贪心)：
### 思路
1. 用哈希表统计$banned$中的数字

2. 然后从小到达的枚举数字$[1,n]$，用$maxSum$减去不在哈希表中的值，直到$maxSum<0$，计算达到条件需要的数字个数


### 复杂度
- 时间复杂度:
  > $O(m+n)$，$n$为给定的值，$m$为$banned$的大小
- 空间复杂度:
  > $O(m)$，哈希表存储

### Code
```C++ []
class Solution {
public:
    int maxCount(vector<int>& banned, int n, int maxSum) {
        unordered_set<int> us;
        for (auto&& x : banned) {
            us.emplace(x);
        }

        int ans = 0;
        for (int i = 1;i <= n;++i) {
            if (us.count(i)) continue;
            maxSum -= i;
            if (maxSum < 0) return ans;
            ++ans;
        }
        return ans;
    }
};
```