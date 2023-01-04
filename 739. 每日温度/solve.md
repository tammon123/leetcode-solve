# 单调栈
## 方法一(单调栈)：
### 思路
1. 我们可以维护一个存储下标的单调栈，因为求下一个更高的温度，从左往右遍历，当前温度比栈顶温度高，说明比栈顶温度高的下一个温度找到了，弹出栈顶温度，因为存入的是下标，可以直接得到距离为$i-下标$，循环比较直到当前温度不比栈顶元素高，然后把当前温度的下标存入栈中
2. 我们默认$ans$数组都为$0$，这样没比较到的，说明没找到，距离为$0$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组temperatures的大小
- 空间复杂度:
  > $O(n)$，栈存储数组temperatures的下标

### Code
```C++ []
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;
        int n = temperatures.size();
        vector<int> ans(n);
        for (int i = 0;i < n;++i) {
            while (!st.empty() && temperatures[st.top()] < temperatures[i]) {
                ans[st.top()] = i - st.top();
                st.pop();
            }
            st.emplace(i);
        }
        return ans;
    }
};
```