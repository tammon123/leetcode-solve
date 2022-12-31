# 栈
## 方法一（栈）：
### 思路
1. 遇到数字入栈，遇到操作符取出栈顶两个数字进行计算，并将结果入栈
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for (auto&& t : tokens) {
            if (t == "+" || t == "-" || t == "*" || t == "/") {
                int num1 = st.top(); st.pop();
                int num2 = st.top(); st.pop();
                if (t == "+") st.emplace(num1 + num2);
                else if (t == "-") st.emplace(num2 - num1);
                else if (t == "*") st.emplace(num1 * num2);
                else st.emplace(num2 / num1);
            }
            else st.emplace(stoi(t));
        }
        return st.top();
    }
};
```
