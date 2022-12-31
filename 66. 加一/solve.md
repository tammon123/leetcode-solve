# 竖式相加
## 方法一（竖式相加）：
### 思路
1. 可以采用竖式相加法，最末尾加一，如果满$10$进位，其他位置同理，最后进位数不为$0$表示还需再开头增加一位$1$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int add = 0, n = digits.size();
        for (int i = n - 1;i >= 0;--i) {
            int r = digits[i] + (i == n - 1 ? 1 : 0) + add;
            digits[i] = r % 10;
            add = r / 10;
        }
        vector<int> ans;
        if (add) {
            ans.emplace_back(add);
        }
        for (int i = 0;i < n;++i) ans.emplace_back(digits[i]);
        return ans;
    }
};
```
