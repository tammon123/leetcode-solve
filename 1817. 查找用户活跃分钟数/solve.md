# 哈希表
## 方法一(哈希表)：
### 思路
1. 用哈希表计算用户活跃分钟数，分钟数用哈希集合去重
2. 然后统计分钟数相同的用户数量计入答案
   

### 复杂度
- 时间复杂度:
  > $O(n+k)$，$n$为数组$logs$的大小，遍历哈希表需要$O(k)$
- 空间复杂度:
  > $O(n)$，哈希表需要$O(n)$空间

### Code
```C++ []
class Solution {
public:
    vector<int> findingUsersActiveMinutes(vector<vector<int>>& logs, int k) {
        unordered_map<int, unordered_set<int>> user_mins;
        for (auto&& log : logs) {
            user_mins[log[0]].emplace(log[1]);
        }
        vector<int> ans(k);
        for (auto&& [_,x] : user_mins) {
            ++ans[x.size() - 1];
        }
        return ans;
    }
};
```