# 前缀和
## 方法一(前缀和)：
### 思路
1. 首先统计范围的和，我们就可以想到前缀和。然后用数组表示元音字母，首尾是元音字母答案加$1$，否则加$0$

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m,n$分别为数组$words,queries$的大小，需要遍历两个数组
- 空间复杂度:
  > $O(m+k)$，前缀和需要存储$m$个数，$k$这里方便处理计为$128$，答案数组不入复杂度

### Code
```C++ []
class Solution {
public:
    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        int m = words.size(), n = queries.size();
        vector<int> prefix_sum(m + 1);
        vector<char> vowel(128);
        vowel['a'] = vowel['o'] = vowel['e'] = vowel['i'] = vowel['u'] = 1;
        for (int i = 0;i < m;++i) {
            string_view s = words[i];
            prefix_sum[i + 1] = prefix_sum[i] + (vowel[s[0]] && vowel[s[s.size() - 1]]);
        }
        vector<int> ans(n);
        for (int i = 0;i < n;++i) {
            auto& x = queries[i];
            ans[i] = prefix_sum[x[1] + 1] - prefix_sum[x[0]];
        }
        return ans;
    }
};
```