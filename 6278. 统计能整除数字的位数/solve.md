# 模拟
## 方法一(模拟)：
### 思路
1. 求出每一位看是否能整除$num$，统计能整除的个数
### 复杂度
- 时间复杂度:
  > $O(n)$，n为num的位数
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int countDigits(int num) {
        int temp = num, ans = 0;
        while (temp) {
            int x = temp % 10;
            if (num % x == 0) ++ans;
            temp /= 10;
        }
        return ans;
    }
};
```