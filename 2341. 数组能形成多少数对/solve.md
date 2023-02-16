# 哈希表
## 方法一(哈希表)：
### 思路
1. 首先想到是，可用哈希表计数，最终成对数是每个元素的数量对$2$求商，不成对是对$2$求余。

2. 但是如果当前元素是$2$的倍数，说明成对，那必定之前统计当前元素为不成对个数需要减一。所以一次遍历，当前元素是$2$的倍数那么成对数加一，不成对数减一，否则不成对数加一。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(n)$，哈希表最多保存$n$个数

### Code
```C++ []
class Solution {
public:
    vector<int> numberOfPairs(vector<int>& nums) {
        vector<int> ans(2);
        unordered_map<int, int> um;
        for (auto&& x : nums) {
            ++um[x];
            if (um[x] % 2 == 0) {
                --ans[1];
                ++ans[0];
            }
            else ++ans[1];
        }
        return ans;
    }
};
```
