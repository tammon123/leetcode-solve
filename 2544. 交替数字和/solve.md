# 模拟
## 方法一(模拟)：
### 思路
1. 因为方便取数据，我们从后往前拆分。如果数字有奇数个，那从后往前与从前往后结果是一样的，如果是偶数个，结果需要取反。例如：
```
521 从前往后与从后往前结果一样 5-2+1=4
5211 从前往后：5-2+1-1=3 从后往前: 1-1+2-5=-3
```

2. 我们用变量$state=1$表示从正数开始，最终只要乘上$-state$就行

### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为给定数，取决于$n$的个数
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int alternateDigitSum(int n) {
        int sum = 0, state = 1;
        while (n) {
            sum += n % 10 * state;
            state = -state;
            n /= 10;
        }
        return sum * -state;
    }
};
```
