# 哈希表/位运算+滑动窗口+哈希表
## 方法一(哈希表)：
### 思路
1. 我们用哈希表存储长度为$k=10$的字符串，并计数。从左往右遍历，我们统计数量等于$2$的为重复出现的，防止重复统计
### 复杂度
- 时间复杂度:
  > $O(nk)$，n为字符串s的大小，k为10
- 空间复杂度:
  > $O(nk)$

### Code
```C++ []
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        unordered_map<string, int> um;
        vector<string> ans;
        string t;
        int n = s.size();
        for (int i = 0;i <= n - k;++i) {
            t = s.substr(i, k);
            if (++um[t] == 2) ans.emplace_back(t);
        }
        return ans;
    }
private:
    const int k = 10;
};
```
## 方法二(位运算+滑动窗口+哈希表)：
### 思路
1. 因为字符串中只有$4$个字母$A、C、G、T$，假设每个字母用二进制$2$位表示，那么$10$个字母共$20$位，可用一个整数存储。设：
    - $A="00"=0$
    - $C="01"=1$
    - $G="10"=2$
    - $T="11"=3$
2. 从左往右遍历，假设当前整数为$x$，右移相当于每次右进一个字母，左出一个字母，那么有：
    - 右进一个字母，$x=(x<<2)|bit[i]$，$bit[i]$表示当前字母映射的二进制
    - 左出一个字母，$x=((x<<2)|bit[i])\&((1<<20)-1)$，其中$((1<<20)-1)$表示$20$位都置为$1$，在与上右进一个字母后的$x$，其他位置都置为$0$了
3. 过程用哈希表存储$x$，计算哈希表等于$2$，防止重复计算
### 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串s的大小
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> ans;
        int n = s.size();
        if (n <= k) return ans;

        int x = 0;
        for (int i = 0;i < k - 1;++i) {
            x = (x << 2) | bit[s[i]];
        }

        unordered_map<int, int> um;
        for (int i = 0;i <= n - k;++i) {
            x = ((x << 2) | bit[s[i + k - 1]]) & ((1 << 20) - 1);
            if (++um[x] == 2) ans.emplace_back(s.substr(i, k));
        }
        return ans;
    }
private:
    const int k = 10;
    unordered_map<char, int> bit = { {'A', 0}, {'C', 1}, {'G', 2}, {'T', 3} };
};
```