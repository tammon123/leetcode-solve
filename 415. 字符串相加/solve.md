# 模拟(竖式相加)
## 思路
1. 我们可以模拟竖式相加过程。从后往前遍历，每一位相加，如果超过$10$，向前进位。我们用个字符串把相加的每一位都存储起来，最后翻转字符串就是答案。
2. 循环停止条件：$num1、num2$都遍历到$0$号位，或者进位数也为$0$。
3. 注意：$num1、num2$任何一个字符串先到了$0$位，还未停止循环条件，后都置为$0$
## 复杂度
- 时间复杂度:
  > $O(max(m,n))$，m为num1的大小，n为num2的大小
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    string addStrings(string num1, string num2) {
        string ans;
        int n = num1.size() - 1, m = num2.size() - 1, add = 0;
        while (n >= 0 || m >= 0 || add != 0) {
            int x = n >= 0 ? num1[n--] - '0' : 0;
            int y = m >= 0 ? num2[m--] - '0' : 0;
            int r = x + y + add;
            ans += '0' + r % 10;
            add = r / 10;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```