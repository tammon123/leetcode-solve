# 竖式相加
## 方法一(竖式相加)：
### 思路
1. 采用竖式相加法，从后往前遍历，数组$num$从$n-1$开始，数字$k$，取从最后一位开始，即$k\%10$。然后两数相加，满$10$进位，假设当前位为$x、y$，上一个进位为$add$，那么就有下一个进位为$\lfloor\frac{x+y+add}{2}\rfloor$，当前位变为$(x+y+add)\ mod\ 10$，数组保存相加后的各个位
2. 最后翻转整个数组
### 复杂度
- 时间复杂度:
  > $O(max(m,n))$，m为数组num的长度，n为数字k的位数
- 空间复杂度:
  > $O(1)$，结果数组不计入空间复杂度

### Code
```C++ []
class Solution {
public:
    vector<int> addToArrayForm(vector<int>& num, int k) {
        int n = num.size() - 1, add = 0;
        vector<int> ans;
        while (n >= 0 || k > 0 || add > 0) {
            int x = n >= 0 ? num[n--] : 0;
            int y = k % 10;
            int r = x + y + add;
            ans.emplace_back(r % 10);
            add = r / 10;
            k /= 10;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```