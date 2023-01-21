# 动态规划
## 方法一(动态规划)：
### 思路
1. 如果字符串由字典组成，那么假设字符串$s[0...i-1]$都是能由字典字符串组成，那么我们只需要考虑$s[i,n-1]$就行。假设$dp[i]$表示字符串前$i$个字符能否被拆分成字典中若干个单词。
2. 那么在$i$之前找到$j$，满足$0<=j<i$，有$dp[j]=true$并且$s[j...i]$在字典中，我们可以先用哈希表存储字典字符串，方便查询
3. 边界条件，空字符串算匹配，那么有$dp[0]=true$

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为字符串$s$的大小,共需要判断$O(n)$个状态，每次有$O(n)$个分割点需要计算
- 空间复杂度:
  > $O(n)$，状态数组需要$O(n)$，哈希表需要$O(n)$

### Code
```C++ []
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> us;
        for (auto&& word : wordDict) us.emplace(word);

        int n = s.size();
        vector<int> dp(n + 1);
        dp[0] = 1;
        for (int i = 1;i <= n;++i) {
            for (int j = 0;j < i;++j) {
                if (dp[j] && us.count(s.substr(j, i - j))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[n];
    }
};
```
## 方法二(剪枝优化动态规划)：
### 思路
1. 假设$dp[i]$表示字符串前$i$个字符能否被拆分成字典中若干个单词。我们可以考虑从字典中判断$dp[i]$是否能组成当前长度字符串，如果不能跳过，直到当前$dp[i]=true$，然后继续在字典中判断$s[i...i+word.size]=word$，说明$dp[i+word.size]=1$。如果$word.size+i>n$可以不用在判断了，进行剪枝。

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为字符串$s$的大小，因为剪枝，复杂度会远小于$O(n^2)$
- 空间复杂度:
  > $O(n)$，状态数组需要$O(n)$

### Code
```C++ []
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int n = s.size();
        vector<int> dp(n + 1);
        dp[0] = 1;
        for (int i = 0;i < n;++i) {
            if (!dp[i]) continue;
            for (auto&& word : wordDict) {
                int size = word.size();
                if (size + i <= n && s.substr(i, size) == word) {
                    dp[i + size] = 1;
                }
            }
        }
        return dp[n];
    }
};
```