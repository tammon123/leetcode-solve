# 位运算
## 方法一(位运算)：
### 思路
1. 要满足强密码条件，首先密码要大于等于$8$位，所以先判断这个条件
2. 用一个变量$pre$表示之前的字符，用来判断当前字符是否与之前字符相等，相等不满足。然后用变量掩码$mask$满足任何其他四个条件，最终$mask=(1<<4)-1$则为强密码，否则不是

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$password$的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool strongPasswordCheckerII(string password) {
        if (password.size() < 8) return false;
        int mask = 0, pre = 0;
        for (auto&& c : password) {
            if (pre == c) return false;
            pre = c;

            if (isdigit(c)) mask |= 1;
            else if (islower(c)) mask |= (1 << 1);
            else if (isupper(c)) mask |= (1 << 2);
            else mask |= (1 << 3);
        }
        return mask == ((1 << 4) - 1);
    }
};
```