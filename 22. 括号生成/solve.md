# 递归+回溯
## 方法一(递归+回溯)：
### 思路
1. $n$个括号，那么最终字符串长度为$2n$的$'('$与$')'$的组合。
2. 我们可以统计当前位置之前$'('$的数量$open$与$')'$的数量$close$
   - 如果$open<n$，那么当前位置可以选择$'('$
   - 如果$close<left$，那么当前位置可以选$')'$，因为一个右括号必定对应一个左括号才是有效的

### 复杂度
- 时间复杂度:
  > $O(\frac{4^n}{\sqrt{n}})$，因为第$n$个数存在卡特兰数$\frac{1}{1+n}\binom{2n}{n}$个组合，渐进为$\frac{4^n}{n\sqrt{n}}$。回溯需要$O(n)$时间
- 空间复杂度:
  > $O(n)$，递归栈深度$O(n)$

### Code
```C++ []
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> ans;
        string temp;
        auto dfs = [&](int cur, int open, int close, auto&& dfs) {
            if (cur == 2 * n) {
                ans.emplace_back(temp);
                return;
            }
            if (open < n) {
                temp.push_back('(');
                dfs(cur + 1, open + 1, close, dfs);
                temp.pop_back();
            }
            if (close < open) {
                temp.push_back(')');
                dfs(cur + 1, open, close + 1, dfs);
                temp.pop_back();
            }
        };
        dfs(0, 0, 0, dfs);
        return ans;
    }
};
```