# 模拟
## 方法一(模拟)：
### 思路
1. 由题目知，税款计算方式如下：

- 不超过$upper0$的收入按税率$percent0$缴纳
- 接着$upper1 - upper0$的部分按税率$percent1$缴纳
- 然后$upper2 - upper1$的部分按税率$percent2$缴纳

2. 当前需要缴纳的税为$(min(income,upper_i)-upper_{i-1})*percent_i$，方便计算，我们令$last=upper_{i-1}$，最初$last=0$。我们只需要计算到$upper_i>=income$，最终要缴纳总款为:$\sum\limits_{upper_i>=income}(min(income,upper_i)-last*percent_i) / 100$
   
   因为$percent_i$不是百分比，我们可以在最后算出结果除以$100$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$brackets$大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    double calculateTax(vector<vector<int>>& brackets, int income) {
        double ans = 0;
        int last = 0;
        for (auto&& bracket : brackets) {
            ans += (min(income, bracket[0]) - last) * bracket[1];
            if (bracket[0] >= income) break;
            last = bracket[0];
        }
        return ans / 100;
    }
};
```
