# 模拟
## 方法一(模拟)：
### 思路
1. 根据题意直接模拟

### 复杂度
- 时间复杂度:
  > $O(nlogk)$，$n$为数组$nums$大小，$k$为$max(nums)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int differenceOfSum(vector<int>& nums) {
        int sum = 0, sum_num = 0;
        auto cal = [&](int num)->int {
            int ret = 0;
            while (num) {
                ret += num % 10;
                num /= 10;
            }
            return ret;
        };
        for (auto&& num : nums) {
            sum += num;
            sum_num += cal(num);
        }
        return abs(sum - sum_num);
    }
};
```
