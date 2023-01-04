# 模拟
## 方法一(模拟)：
### 思路
1. 我们可以反向遍历，假设开始变量$pre$等于空字符，如果当前字符还是为空，继续遍历。如果不为空字符，则为字母，计数加一，并让$pre$等于当前字符
2. 如果当前字符为空字符且$pre$不为空字符，说明最后一个单词长度已经读取完毕返回计数
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int lengthOfLastWord(string s) {
        int n = s.size(), ans = 0;
        char pre = ' ';
        for (int i = n - 1;i >= 0;--i) {
            if (pre == ' ' && s[i] == ' ') continue;
            else if (s[i] != ' ') ++ans, pre = s[i];
            else if (pre != ' ' && s[i] == ' ') return ans;
        }
        return ans;
    }
};
```