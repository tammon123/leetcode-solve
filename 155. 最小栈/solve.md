# 辅助线
## 思路
1. 我们用一个辅助栈用来存储最小值，开始插入$INT_MAX$，之后的每次插入都插入与最小的，即让最小值的栈属于非递增栈。这样就能保证，每次获取最小值都是可以$O(1)$时间从最小值栈顶元素取值。
## 复杂度
- 时间复杂度:
  > $O(1)$，栈的所有操作都是O(1)时间复杂度
- 空间复杂度:
  > $O(n)$，n为操作次数

## Code
```C++ []
class MinStack {
public:
    MinStack() {
        m_minst.emplace(INT_MAX);
    }

    void push(int val) {
        m_st.emplace(val);
        m_minst.emplace(min(m_minst.top(), val));
    }

    void pop() {
        m_st.pop();
        m_minst.pop();
    }

    int top() {
        return m_st.top();
    }

    int getMin() {
        return m_minst.top();
    }
private:
    stack<int> m_st;
    stack<int> m_minst;
};
```