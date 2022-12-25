# 栈+哈希表
## 思路
1. 有效括号字符，只要包含$"("、")"$那左右括号应该成对出现，并且右括号只会与左边最近的括号匹配，没有匹配不算有效括号。
2. 我们只需要用栈存储左括号的下标，遇到右括号就弹出栈顶的左括号。如果遇到右括号，栈为空了，说明这个右括号要删除。假如遍历完了字符串，栈中还有左括号，说明这些左括号没有匹配到右括号，也要删除。
3. 那么我在遍历过程可以用哈希表存储要删除的括号元素下标，在遍历一遍字符串，我们构建新字符串，遇到删除下标就不添加，反之添加。
## 复杂度
- 时间复杂度:
  > $O(n)$，n为字符串长度
- 空间复杂度:
  > $O(n)$，哈希表要存储的字符下标

## Code
```C++ []
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        int n = s.size();
        stack<int> st;
        unordered_set<int> us;
        for (int i = 0;i < n;++i) {
            if (s[i] == '(') st.emplace(i);
            else if (s[i] == ')') {
                if (st.empty()) us.emplace(i);
                else st.pop();
            }
        }

        while (!st.empty()) { us.emplace(st.top()); st.pop(); }
        string ans;
        for (int i = 0;i < n;++i) {
            if (us.count(i)) continue;
            else ans += s[i];
        }
        return ans;
    }
};
```