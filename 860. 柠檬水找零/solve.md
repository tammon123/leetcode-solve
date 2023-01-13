# 分类讨论+贪心
## 方法一(分类讨论+贪心)：
### 思路
1. 我们只会用$5$美元与$10$美元找零，假设$5$美元数量为$five$，$10$美元数量为$ten$，我们可以分类讨论：
   - 如果收到$5$美元，不用找零，$five$加一
   - 如果收到$10$美元，判断$five>0$，$five$减一，$ten$加一。否则不能找零，返回$false$
   - 如果收到$20$美元，那么找零组合可以为$10+5$或者$3*5$，满足找零条件情况下，优先选择$10+5$组合，因为$10$可以由两个$5$组合成，$5$的需求比$10$大，所以优先把$10$找零出去

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$bills$大小
- 空间复杂度:
  > $O(1)$，只有两个变量

### Code
```C++ []
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int five = 0, ten = 0;
        for (auto&& bill : bills) {
            if (bill == 5) ++five;
            else if (bill == 10) {
                if (five > 0) --five, ++ten;
                else return false;
            }
            else {
                if (ten >= 1 && five >= 1) {
                    --ten;
                    --five;
                }
                else if (five >= 3) five -= 3;
                else return false;
            }
        }
        return true;
    }
};
```