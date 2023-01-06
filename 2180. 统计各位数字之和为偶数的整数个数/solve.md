# 模拟
## 方法一(模拟)：
### 思路
1. 从$1$开始遍历到$num$，对每个数字求各位之和，判断是否为偶数
### 复杂度
- 时间复杂度:
  > $O(num*lognum)$，遍历num个正整数，求各位之和复杂度为lognum
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int countEven(int num) {
        auto check = [&](int x)->bool {
            int sum = 0;
            while (x) {
                sum += x % 10;
                x /= 10;
            }
            return !(sum & 1);
        };
        int ans = 0;
        for (int i = 1;i <= num;++i) {
            if (check(i)) ++ans;
        }
        return ans;
    }
};
```