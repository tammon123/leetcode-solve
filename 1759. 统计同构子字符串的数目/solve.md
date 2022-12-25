# 数学
## 思路
1. 在子字符串中，假如连续两个不为同字母，只需要统计一次，如果为同字母，是数列级增长

例如：
```
a b b c c c a a
1 1 2 1 2 3 1 2 这里每一位代表增长数量，最终为全部数和13
b->bb 
1->2
c->cc->ccc
1->2->3
```
## 复杂度
- 时间复杂度:
  > $O(n)$，n字符串长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    static const int MOD = 1e9 + 7;
    int countHomogenous(string s) {
        int cnt = 1, ans = 1, n = s.size();
        for (int i = 1;i < n;++i) {
            if (s[i] == s[i - 1]) {
                cnt = cnt % MOD + 1;
                ans = ((long)(ans + cnt) % MOD);
            }
            else {
                cnt = 1;
                ans = (long(ans + 1) % MOD);
            }
        }
        return ans % MOD;
    }
};
```