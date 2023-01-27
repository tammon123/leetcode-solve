# 动态规划
## 方法一(动态规划)：
### 思路
1. 当字符大小大于$2$时，一个字符想要是回文串，那么它去掉首尾后，仍然是回文串。那么我们可以用动态规划的思想，假设$dp[i][j]$表示字符串$s[i...j]$是否为回文串，那么有：
   - 如果$s[i]=s[j]$，那么需要查看$s[i+1..j-1]$是否为回文串，有$dp[i][j]=dp[i+1][j-1]$
   - 如果$s[i]\neq s[j]$，$s[i...j]$不是回文串，有$dp[i][j]=0$

2. 这些都是在字符大于$2$的时候，我们考虑字符只有$1$，那么字符串必定是回文串，有:$dp[i][i]=1$。如果字符串有$2$个字符的时候，只需要判断$s[i]=s[j]$为回文串，否则不是

3. 我们可以按照字符长度递增迭代字符串，自底向上的动态规划
### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为字符串$s$的长度
- 空间复杂度:
  > $O(n^2)$，需要存储一个大小为$n^2$的状态数组

### Code
```C++ []
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size(), maxLen = 1, start = 0;
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 0;i < n;++i) dp[i][i] = 1;

        for (int L = 2;L <= n;++L) {
            for (int i = 0;i < n;++i) {
                int j = i + L - 1;
                if (j >= n) break;

                if (s[i] != s[j]) dp[i][j] = 0;
                else {
                    if (L <= 2) dp[i][j] = 1;
                    else dp[i][j] = dp[i + 1][j - 1];
                }

                if (dp[i][j] && L > maxLen) {
                    maxLen = L;
                    start = i;
                }
            }
        }
        return s.substr(start, maxLen);
    }
};
```

## 方法二(中心扩展法)：
### 思路
1. 由方法一我们可以看到，要计算$dp[i][j]$，需要考虑$dp[i+1][j-1]$，一直往下下去直到边界。那么我们可以考虑由边界往两边扩展，一直扩展到两边字符不相等位置，统计这时候的长度。那么所有边界遍历结束，其中最长的回文串就是我们要求的。
   
2. 边界从哪里开始了，需要考虑$1$跟$2$的情况。
### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为字符串$s$的长度。长度为$1$跟$2$的回文中心有$n$与$n-1$个，每个回文需要扩展$O(n)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size(), start = 0, end = 0;
        auto expand = [&](int left, int right)->pair<int, int> {
            while (left >= 0 && right < n && s[left] == s[right]) {
                --left;
                ++right;
            }
            return { left + 1,right - 1 };
        };

        for (int i = 0;i < n;++i) {
            auto [left1, right1] = expand(i, i);
            auto [left2, right2] = expand(i, i + 1);
            if (right1 - left1 > end - start) {
                start = left1;
                end = right1;
            }
            if (right2 - left2 > end - start) {
                start = left2;
                end = right2;
            }
        }
        return s.substr(start, end - start + 1);
    }
};
```