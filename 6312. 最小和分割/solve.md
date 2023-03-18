# 思维题+排序
## 方法一(思维题+排序)：
### 思路
1. 首先拆分的$num1,num2$最终组成应该都是$num$的数字，可以包含前导零。要想两个数字最终和最小，我们想到应该均匀分配给$num1,num2$，即按从小到大顺序，$num1,num2$依次挑选数字组合。

2. 那么我们先把数字转换成字符串进行排序，然后按顺序挑选，最终求和就行。


### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数字$num$的长度，排序需要$O(nlogn)$，求和需要$O(n)$,最终为$O(nlogn)$
- 空间复杂度:
  > $O(logn)$，排序需要栈空间

### Code
```C++ []
class Solution {
public:
    int splitNum(int num) {
        string s = to_string(num);
        int n = s.size(), ans = 0;
        sort(s.begin(), s.end());

        int num1 = 0, num2 = 0;
        for (int i = 0;i < n;++i) {
            int x = s[i] - '0';
            if ((i & 1)) num2 = 10 * num2 + x;
            else num1 = 10 * num1 + x;
        }
        return num1 + num2;
    }
};
```