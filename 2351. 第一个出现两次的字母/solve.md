# 哈希表/位运算
## 方法一(哈希表)：
### 思路
1. 用哈希表存储字符，如果在哈希表中查询有重复，返回重复字符
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串长度
- 空间复杂度:
  > $O(k)$，这里为26

### Code
```C++ []
class Solution {
public:
    char repeatedCharacter(string s) {
        unordered_set<char> us;
        for (auto&& c : s) {
            if (us.count(c)) return c;
            us.emplace(c);
        }
        return 0;
    }
};
```
## 方法二(位运算)：
### 思路
1. 因为只有小写字母，我们可以让每个字母对应二进制一位，新的字符**与**上掩码后如果还等于新的字符，说明之前已经存在过这个字符了，返回这个字符
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    char repeatedCharacter(string s) {
        int mask = 0;
        for (auto&& c : s) {
            int n = 1 << (c - 'a');
            if ((mask & n) == n) return c;
            mask |= n;
        }
        return 0;
    }
};
```