# 贪心
## 方法一(贪心)：
### 思路
1. 我们只需要按位模拟就行，划分的子串尽可能的大，用一个计数变量$t$(因为$k$比较大，所以取$long long$)，从左往右遍历，$t$乘$10$加上当前位数，与$k$比较大小：
    - 如果满足$t*10+t2<=k$，继续往右遍历
    - 如果$t*10+t2>k$，判断$t<=k$，可以进行分割，答案加一，并令$t=t2$，否则不能分割，返回$-1$，例如:```238182，k=5```，$8>5$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minimumPartition(string s, int k) {
        long long t = 0;
        int ans = 0;
        for (auto&& c : s) {
            int t2 = c - 48;
            if ((t * 10 + t2) <= k) {
                t = t * 10 + t2;
            }
            else {
                if (t <= k) {
                    ++ans;
                    t = t2;
                }
                else return -1;
            }
        }
        if (t <= k) ++ans;
        return ans;
    }
};
```