# 模拟
## 方法一(模拟)：
### 思路
1. 构建两个字符串，遍历字符串$s、t$，如果遇到$#$，字符串就退格一个字符，如果字符串为空不退格，反之存入字母
2. 最后比较两个字符串是否相等
### 复杂度
- 时间复杂度:
  > $O(m+n)$，m为字符串s的长度，n为字符串t的长度
- 空间复杂度:
  > $O(m+n)$，构建两个字符串长度最大为m+n

### Code
```C++ []
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        string ans, ans2;
        for (auto&& c : s) {
            if (c == '#' && !ans.empty()) ans.pop_back();
            else if (c != '#') ans.push_back(c);
        }

        for (auto&& c : t) {
            if (c == '#' && !ans2.empty()) ans2.pop_back();
            else if (c != '#') ans2.push_back(c);
        }

        return ans == ans2;
    }
};
```
## 方法二(双指针)：
### 思路
1. 我们从后往前遍历，如果遇到$'#'$就计算需要往前的字母数，用双指针分别指向两个字符串的最后位
2. 如果当前字母不为$'#'$，且需要跳过的字母数为$0$，我们就对比两指针所指的字母，如果相等继续往下遍历，如果不相等或者有一个提前遍历结束，说明不匹配，返回$false$
### 复杂度
- 时间复杂度:
  > $O(m+n)$，m为字符串s的长度，n为字符串t的长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        int m = s.size(), n = t.size(), i = m - 1, j = n - 1, skip_s = 0, skip_t = 0;
        while (i >= 0 || j >= 0) {
            while (i >= 0) {
                if (s[i] == '#') {
                    ++skip_s;
                    --i;
                }
                else if (skip_s > 0) {
                    --skip_s;
                    --i;
                }
                else break;
            }

            while (j >= 0) {
                if (t[j] == '#') {
                    ++skip_t;
                    --j;
                }
                else if (skip_t > 0) {
                    --skip_t;
                    --j;
                }
                else break;
            }

            if (i >= 0 && j >= 0) {
                if (s[i] != t[j]) return false;
            }
            else {
                if (i >= 0 || j >= 0) return false;
            }
            --i;--j;
        }
        return true;
    }
};
```