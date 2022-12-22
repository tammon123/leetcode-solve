# 哈希表
## 思路
1. 题目要求最长回文串，没有要求顺序。我们可先遍历，用哈希表存储各个字母的个数。
2. 假如有偶数个字母$a$，为了使回文串最大，全部选择；假如有奇数个字母$b$，我们只需选择奇数个$b$减$1$。最终奇数字母个数大于$0$，再多选$1$位，那最终个数再加$1$；奇数个数等于$0$，意味着都是偶数，那不用再多选$1$位。
## 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的大小
- 空间复杂度:
  > $O(n)$， 哈希表大小

## Code
```C++ []
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char, int> um;
        for (auto&& c : s) ++um[c];
        bool hasodd = false;
        int ans = 0;
        for (auto&& [_, cnt] : um) {
            if (cnt & 1) hasodd = true;
            ans += cnt & 1 ? cnt - 1 : cnt;
        }
        ans += hasodd;
        return ans;
    }
};
```