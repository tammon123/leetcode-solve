# 贪心
## 方法一(贪心)：
### 思路
1. 要组成给定值的最小字符串，假设当前第$i$位字符的值是$c$，那么后面还需要$n-i$位字符，还剩值为${k}'-c$，每一位的最小值为$'a'=1$，最大值为$'z'=26$，有:
   
   $(n-i)<={k}'-c<=(n-i)*26 <=> {k}'-(n-i)*26<=c<={k}'-(n-i)$

2. 那么按照贪心思想，每次取最小值$c=max(1,{k}'-(n-i)*26)$，即：${k}'-(n-i)*26>0$，取它。反之取$'a'=1$
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$表示给定的数字$n$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    string getSmallestString(int n, int k) {
        string ans;
        for (int i = 1;i <= n;++i) {
            int low = max(1, k - (n - i) * 26);
            k -= low;
            ans.push_back('a' + low - 1);
        }
        return ans;
    }
};
```