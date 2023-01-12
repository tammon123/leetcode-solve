# 哈希表
## 方法一(哈希表)：
### 思路
1. 首先我们用哈希表计入键值
2. 因为最终返回字符串，我们可以模拟栈过程，遇到字符$')'$，末尾弹出到字符$'('$，包含$'('$，用临时字符串变量记录弹出的字符，然后翻转字符串再哈希表中查找，如果匹配，答案字符串加入值，反之加入字符$'?'$

### 复杂度
- 时间复杂度:
  > $O(m+n+k)$，$n$为字符串$s$的长度，$m$为数组$knowledge$的大小，k为数组$knowledge$中字符串长度总和
- 空间复杂度:
  > $O(m+k)$，哈希表存储数组$knowledge$的大小，临时变量为数组$knowledge$中字符串长度总和

### Code
```C++ []
class Solution {
public:
    string evaluate(string s, vector<vector<string>>& knowledge) {
        unordered_map<string, string> um;
        for (auto&& x : knowledge) {
            um[x[0]] = x[1];
        }

        string ans;
        int n = s.size();
        for (int i = 0;i < n;++i) {
            if (s[i] == ')') {
                string t;
                while (!ans.empty() && ans.back() != '(') {
                    t += ans.back();
                    ans.pop_back();
                }
                if (!ans.empty()) ans.pop_back();
                reverse(t.begin(), t.end());
                if (um.count(t)) ans += um[t];
                else ans += '?';
            }
            else ans += s[i];
        }
        return ans;
    }
};
```
## 方法二(哈希表，优化入栈过程)：
### 思路
1. 同样先用哈希表计入键值
2. 然后用变量$eval$判断遇到$'('$设为$true$，然后统计往后的字符放入临时字符串中，知道遇到$')'$。对比临时字符串在哈希表中是否有值，有插入答案字符串中，没有把字符$'?'$插入字符串中

### 复杂度
- 时间复杂度:
  > $O(m+n+k)$，$n$为字符串$s$的长度，$m$为数组$knowledge$的大小，k为数组$knowledge$中字符串长度总和
- 空间复杂度:
  > $O(m+k)$，哈希表存储数组$knowledge$的大小，临时变量为数组$knowledge$中字符串长度总和

### Code
```C++ []
class Solution {
public:
    string evaluate(string s, vector<vector<string>>& knowledge) {
        unordered_map<string, string> um;
        for (auto&& x : knowledge) {
            um[x[0]] = x[1];
        }

        string ans, t;
        int n = s.size();
        bool eval = false;
        for (int i = 0;i < n;++i) {
            if (s[i] == '(') eval = true;
            else if (s[i] == ')') {
                eval = false;
                if (um.count(t)) ans += um[t];
                else ans += '?';
                t.clear();
            }
            else if (eval) t += s[i];
            else ans += s[i];
        }
        return ans;
    }
};
```