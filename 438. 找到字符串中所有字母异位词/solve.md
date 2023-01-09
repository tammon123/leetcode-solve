# 哈希表+滑动窗口
## 方法一(哈希表+滑动窗口)：
### 思路
1. 先用哈希表存入字符串$p$的字母数，假设$p$中有$n$个字母
2. 匹配字符串$s$，维护一个滑动窗口，前$n$个匹配一次字符串$p$，后往右遍历一位，滑动窗口最左会出一位，然后统计新的窗口对应的字符串与$p$匹配
### 复杂度
- 时间复杂度:
  > $O(n+(m-n)*k)$，m为字符串s的长度，n为字符串p的长度，需要判断m-n+1个滑动窗口字母数量与字符串p匹配，k为26
- 空间复杂度:
  > $O(k)$，存储小写字母，k为26

### Code
```C++ []
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int m = s.size(), n = p.size();

        unordered_map<int, int> um, check;
        for (auto&& c : p) {
            ++check[c - 'a'];
        }

        vector<int> ans;
        for (int i = 0;i < m;++i) {
            if (i + 1 >= n) {
                ++um[s[i] - 'a'];
                if (i + 1 > n) --um[s[i - n] - 'a'];
                bool match = true;
                for (auto&& [c, cnt] : check) {
                    if (um[c] != cnt) {
                        match = false;
                        break;
                    }
                }
                if (match) ans.emplace_back(i - n + 1);
            }
            else ++um[s[i] - 'a'];
        }
        return ans;
    }
};
```
## 方法二(优化滑动窗口)：
### 思路
1. 我们统计滑动窗口与字符串$p$不同字母的个数，并用变量$diff$记录滑窗与字符串$p$不同字母个数，再滑动过程维护整个变量。
2. 滑窗过程变化：
   - 要退出滑窗的字母个数为$1$时，说明之前不匹配字母，要变匹配了变量$diff$减一，要退出滑窗字母个数为$0$时，说明之前匹配字母要变不匹配了，$diff$加一
   - 要进入滑窗的字母个数为$-1$时，说明之前不匹配字母要匹配了，$diff$减一，进入滑窗字母个数为$0$，说明之前匹配的要不匹配了，$diff$加一
   - 再滑动窗口过程中，匹配只需要判断$diff=0$
### 复杂度
- 时间复杂度:
  > $O(m+n+k)$，m为字符串s的长度，n为字符串p的长度，我们需要$O(n)$来初始化滑动窗口，$O(m)$遍历字符串s，$O(k)$初始化diff大小，k为26个小写字母
- 空间复杂度:
  > $O(k)$

### Code
```C++ []
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int m = s.size(), n = p.size();
        if (m < n) return {};
        vector<int> check(26), ans;
        int diff = 0;
        for (int i = 0;i < n;++i) {
            ++check[s[i] - 'a'];
            --check[p[i] - 'a'];
        }

        for (int i = 0;i < 26;++i) {
            if (check[i] != 0) ++diff;
        }
        if (diff == 0) ans.emplace_back(0);

        for (int i = 0;i < m - n;++i) {
            if (check[s[i] - 'a'] == 1) --diff;
            else if (check[s[i] - 'a'] == 0) ++diff;
            --check[s[i] - 'a'];

            if (check[s[i + n] - 'a'] == 0) ++diff;
            else if (check[s[i + n] - 'a'] == -1) --diff;
            ++check[s[i + n] - 'a'];

            if (diff == 0) ans.emplace_back(i + 1);
        }
        return ans;
    }
};
```