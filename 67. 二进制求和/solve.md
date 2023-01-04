# 竖式相加
## 方法一(竖式相加)：
### 思路
1. 采用竖式相加法，往右对齐，从后往前遍历，满$2$进位$1$，假设当前位为$x、y$，上一个进位为$add$，那么就有下一个进位为$\lfloor\frac{x+y+add}{2}\rfloor$，当前位变为$(x+y+add)\ mod\ 2$，用字符串连接相加后的各位
2. 最后翻转整个字符串
### 复杂度
- 时间复杂度:
  > $O(max(m,n))$，m为字符串a的长度，n为字符串b的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    string addBinary(string a, string b) {
        int m = a.size() - 1, n = b.size() - 1, add = 0;
        string ans;
        while (m >= 0 || n >= 0 || add > 0) {
            int x = m >= 0 ? a[m--] - '0' : 0;
            int y = n >= 0 ? b[n--] - '0' : 0;
            int r = x + y + add;
            ans += r % 2 + '0';
            add = r / 2;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```