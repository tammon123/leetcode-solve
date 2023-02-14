# 滑动窗口
## 方法一(滑动窗口)：
### 思路
1. 首先我们需遍历一遍数组，然后统计四个字符的数量，如果所有字符数量都等于$balance\_cnt=n/4$，那么不需要替换，返回$0$。

2. 如果不平衡，我们可以考虑最终平衡需要替换几个字符，那么子串肯定包含这几个字符，例如:```WWEQERQWQWWRWWERQWEQ```，需要替换$3$个```W```转化为其他缺少的字符，那么我们需要怎么找到最短替换子串呢。可以用滑动窗口思想，先从左到右遍历滑动右窗口，子串中包含三个```W```的第一个子串为```WWEQERQW```，统计长度，然后滑动左窗口到不平衡```WEQERQW```，继续滑动右窗口。一直这样迭代，最终找到最短替换子串长度为$4$，为```WQWW、WWRW```等等都可以。

3. 我们可以数组统计需要替换的字符，然后用另外一个数组滑动，过程比较两个数组。

优化过程3：我们可以利用最开始统计的数组，滑动右窗口，减去数量，让子串达到平衡记录长度。否则滑动左窗口让子串不平衡。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$s$的长度，需要遍历统计一遍数量$O(n)$，滑动窗口需要遍历数组$O(n)$
- 空间复杂度:
  > $O(k)$，这里$k$方便统计为$128$，没优化哈希表数据不会超过$128$，最终为$k$

没优化代码：
### Code
```C++ []
class Solution {
public:
    int balancedString(string s) {
        int n = s.size(), balance_cnt = n / 4;
        vector<int> basecnt(128);
        vector<char> base{'W', 'Q', 'E', 'R'};
        for (auto&& c : s) {
            ++basecnt[c];
        }
        unordered_map<char, int> cnt, sliding;
        for (int i = 0;i < 4;++i) {
            if (basecnt[base[i]] > balance_cnt) {
                cnt[base[i]] = basecnt[base[i]] - balance_cnt;
            }
        }

        auto check = [&]()->bool {
            for (auto& [x, y] : cnt) {
                if (sliding[x] < y) return false;
            }
            return true;
        };
        int ans = INT_MAX, j = 0;
        if (check()) return 0;
        for (int i = 0;i < n;++i) {
            ++sliding[s[i]];
            while (j <= i && check()) {
                ans = min(ans, i - j + 1);
                --sliding[s[j++]];
            }
        }
        return ans == INT_MAX ? 0 : ans;
    }
};
```
优化后代码：

### Code
```C++ []
class Solution {
public:
    int balancedString(string s) {
        int n = s.size(), balance_cnt = n / 4;
        vector<int> cnt(128);
        for (auto&& c : s) ++cnt[c];

        auto check = [&]()->bool {
            if (cnt['W'] <= balance_cnt && cnt['Q'] <= balance_cnt
                && cnt['E'] <= balance_cnt && cnt['R'] <= balance_cnt) {
                return true;
            }
            return false;
        };
        int ans = n, j = 0;
        if (check()) return 0;
        for (int i = 0;i < n;++i) {
            --cnt[s[i]];
            while (j <= i && check()) {
                ans = min(ans, i - j + 1);
                ++cnt[s[j++]];
            }
        }
        return ans;
    }
};
```