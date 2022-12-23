# 哈希表
## 思路
1. 题目要求$s$按照$pattern$完全匹配，我们可以用两个哈希表分别映射$s$每段跟$pattern$的每个字母。
2. 例如：```pattern = "abba", s = "dog cat cat dog"```中，$a$映射$dog$，$dog$映射$a$。如果$pattern$跟$s$任何一个没完全匹配，返回$false$；如果字母$a$映射$dog$，又有字母$a$映射$cat$，或者字母$b$映射$dog$，以上情况都返回$false$

## 复杂度
- 时间复杂度:
  > $O(m+n)$，m为pattern长度，n为s长度
- 空间复杂度:
  > $O(m+n)$

## Code
```C++ []
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        int m = pattern.size(), n = s.size(), p = 0;
        if (m > n) return false;
        unordered_map<char, string> um;
        unordered_map<string, char> um2;
        for (int i = 0;i < m;++i) {
            string t;
            while (p < n) {
                if (s[p] == ' ') { ++p; break; }
                t += s[p++];
            }
            if(p==n && i!=m-1) return false;
            if (i == 0) { um[pattern[i]] = t; um2[t] = pattern[i]; continue; }
            if (um.count(pattern[i]) && um[pattern[i]] != t) return false;
            if (!um.count(pattern[i]) && um2.count(t)) return false;
            um[pattern[i]] = t; um2[t] = pattern[i];
        }
        if(p!=n) return false;
        return true;
    }
};
```