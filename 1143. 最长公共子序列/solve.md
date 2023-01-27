# 动态规划
## 思路
1. 我们设$dp[i][j]$表示$text1[0...i-1]$、$text2[0...j-1]$的最长公共子序列的长度
2. 当$i=0$时，表示$text1$空字符串跟$text2[0...j-1]$比较，没有公共子序列，所以$dp[0][j]=0$；同理$j=0$时，$dp[i][0]=0$
3. 当$i>0 \&\& j>0$时
- 如果$text1[i-1]=text2[j-1]$，最长公共子序列加$1$，$dp[i][j]=dp[i-1][j-1]+1$
- 如果$text1[i-1]\neq text2[j-1]$，需要考虑：
    - $text1[0...i-2]$与$text2[0...j-1]$的最长公共子序列
    - $text1[0...i-1]$与$text2[0...j-2]$的最长公共子序列

由此得到状态转移公式：

$\left\{
    \begin{array} {l}
        dp[i][j]=dp[i-1][j-1]+1，&text1[i-1]=text2[j-1] \\
        dp[i][j]=max(dp[i-1][j], dp[i][j-1])，&text1[i-1]\neq text2[j-1]
    \end{array}
\right.$

那么最长公共子序列长度为$dp[m][n]$
### 复杂度
- 时间复杂度:
  > $O(mn)$，其中$m,n$分别为字符串$text1,text2$的长度
- 空间复杂度:
  > $O(mn)$，二维数组$dp$

### Code
```C++ []
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                if (text1[i - 1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[m][n];
    }
};
```