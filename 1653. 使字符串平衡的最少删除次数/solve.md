# 动态规划
## 方法一(动态规划)：
### 思路
1. 由题目知道，最终删除完毕后最终形态是所有$'a'$应该在$'b'$之前。我们可以考虑动态规划思想，如果当前字符是$'a'$，那么以$'a'$结尾只需要考虑上一个$'a'$结尾的长度加一。如果当前字符是$'b'$，需要考虑上一个字符是$'a'或'b'$结尾长度中最大的长度加一，最终求出$'a'或'b'$长度最大值，删除元素就为总长减去这个值。
   
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$s$的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minimumDeletions(string s) {
        int a = 0, b = 0, n = s.size();
        for (auto&& x : s) {
            if (x == 'a') a += 1;
            else b = max(a, b) + 1;
        }
        return n - max(a, b);
    }
};
```
## 换一种思考方式

### 思路
1. 另一种动态规划思想，我们遇到$'b'$不考虑删除，遇到$'a'$考虑删除或者删除前面$'b'$的数量，每次取删除的最小值，最终结果就是要删除的最小数。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$s$的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minimumDeletions(string s) {
        int bCnt = 0, ans = 0;
        for (auto&& x : s) {
            if (x == 'a') ans = min(ans + 1, bCnt);
            else ++bCnt;
        }
        return ans;
    }
};
```