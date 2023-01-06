# 双指针
## 方法一(双指针)：
### 思路
1. 下一个排列做法，将数字转为字符串，用双指针，从后往前遍历，首先指针$i$指向第一个小于后一位的数，如果$i<0$说明数字排列已经是最大，返回$-1$
2. 然后指针$j$同样从后往前遍历，找到第一个大于$nums[i]$的数，交换$nums[i]$于$nums[j]$
3. 最后翻转字符$[i+1, n-1]$的数
### 复杂度
- 时间复杂度:
  > $O(logn)$，n为数字位数
- 空间复杂度:
  > $O(logn)$

### Code
```C++ []
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);
        int i = s.size() - 2;
        while (i >= 0 && s[i] >= s[i + 1]) --i;
        if (i < 0) return -1;

        int j = s.size() - 1;
        while (j >= 0 && s[i] >= s[j]) --j;
        
        swap(s[i], s[j]);
        reverse(s.begin() + i + 1, s.end());
        long ans = stol(s);
        return ans > INT_MAX ? -1 : ans;
    }
};
```
