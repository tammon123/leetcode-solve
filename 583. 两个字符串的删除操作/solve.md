# 最长公共子序列/动态规划
## 方法一(最长公共子序列)：
### 思路
1. 参考[1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)
2. 那么要删除字符数就需要两个字符串长度总长减去$2$倍的最长公共子序列长度

### 复杂度
- 时间复杂度:
  > $O(mn)$，其中$m,n$分别为字符串$text1,text2$的长度。二维数组$dp$需要对$dp$中每个元素进行计算
- 空间复杂度:
  > $O(mn)$，二维数组$dp$

### Code
```C++ []
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
        return m + n - 2 * dp[m][n];
    }
};
```

## 方法二(动态规划)：
### 思路
1. 我们也不需要计算最长公共子序列。假设$dp[i][j]$表示$word1[0...i-1]$和$word2[0...j-1]$相同的最少删除操作次数。
2. 边界情况，当$i=0$时，$word1$表示空字符串，那么任意长度的$word2$要与它匹配，必须删除整个$word2$，所以$dp[0][j]=j$。同理当$j=0$时，$dp[i][0]=i$
3. 当$i>0\&\&j>0$时：
   - 如果$text[i-1]=text[j-1]$，说明公共字符，不需要删除，有$dp[i][j]=dp[i-1][j-1]$
   - 如果$text[i-1]\neq text[j-1]$，需要考虑两种情况：
        - 考虑$text[0...i-2]$和$text[0...j-1]$相同的最少删除操作次数，加上这次不同的删除数加一
        - 考虑$text[0...i-1]$和$text[0...j-2]$相同的最少删除操作次数，加上这次不同的删除数加一
        - 要使得删除次数最少，应该取两种情况中较小的一项，有: $dp[i][j]=min(dp[i-1][j],dp[i][j-1])+1$
4. 最终转移方程为：
   $\left\{
    \begin{array} {l}
        dp[i][j]=dp[i-1][j-1]，&text1[i-1]=text2[j-1] \\
        dp[i][j]=max(dp[i-1][j], dp[i][j-1])+1，&text1[i-1]\neq text2[j-1]
    \end{array}
\right.$

### 复杂度
- 时间复杂度:
  > $O(mn)$，其中$m,n$分别为字符串$text1,text2$的长度。二维数组$dp$需要对$dp$中每个元素进行计算
- 空间复杂度:
  > $O(mn)$，二维数组$dp$
### Code
```C++ []
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int i = 1;i <= m;++i) {
            dp[i][0] = i;
        }
        for (int j = 1;j <= n;++j) {
            dp[0][j] = j;
        }

        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                if (word1[i - 1] == word2[j - 1]) dp[i][j] = dp[i - 1][j - 1];
                else dp[i][j] = min(dp[i][j - 1], dp[i - 1][j]) + 1;
            }
        }
        return dp[m][n];
    }
};
```