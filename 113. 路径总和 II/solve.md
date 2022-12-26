# 贪心
## 思路
1. 因为一次操作能改变三个位置，那么我们让每次操作处于中间位置肯定是最少次数，即：假设当前位置为$i$，那么只要前一个字母为$'X'$就可以进行一次操作，使得$s[i-1]、s[i+1]$都变为$'O'$
2. 注意：$i>0\And i+1<n$，且遍历结束，最后一个字母是$'X'$，答案加一，例如：$"XOOX"$
## 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的大小
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    int minimumMoves(string s) {
        int ans = 0, n = s.size();
        for (int i = 0;i < n;++i) {
            if (i > 0 && s[i - 1] == 'X') {
                s[i - 1] = s[i] = 'O';
                if (i + 1 < n) s[i + 1] = 'O';
                ++ans;
            }
        }
        return s[n - 1] == 'X' ? ans + 1 : ans;
    }
};
```