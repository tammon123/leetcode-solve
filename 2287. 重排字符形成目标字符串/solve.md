# 哈希表
## 方法一(哈希表)：
### 思路
1. 字符串$s$任意位置拿出的新字符串能重组成字符串$target$，数量加一
2. 用过的字符串$s$的字符就不能再用，那么我们只需用哈希表统计两个字符串的每个字符数量，然后遍历$target$哈希表：
   - 如果$s$哈希表没有$target$字符，返回$0$
   - 有就返回相应字符比例最小的比值

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m$为字符串$s$的大小，$n$为字符串$target$的大小
- 空间复杂度:
  > $O(max(m,n))$，哈希表最大存储为$O(max(m,n))$

### Code
```C++ []
class Solution {
public:
    int rearrangeCharacters(string s, string target) {
        unordered_map<char, int> um_t, um_s;
        for (auto&& c : target) ++um_t[c];
        for (auto&& c : s) ++um_s[c];

        int ans = INT_MAX;
        for (auto& [c, cnt] : um_t) {
            if (!um_s.count(c)) return 0;
            ans = min(ans, um_s[c] / cnt);
        }
        return ans;
    }
};
```