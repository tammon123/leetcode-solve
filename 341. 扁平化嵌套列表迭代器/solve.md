# 栈
## 方法一(栈)：
### 思路
1. 嵌套的整形列表是树形结构，我们可以采用栈迭代，栈中存储列表的首尾迭代器。
   - 初始化，可以将列表首尾存入栈中
   - $hasNext:$迭代栈，判断栈顶元素的首尾迭代器是否相等，相等说明遍历结束，弹出栈。不相等可以判断开始迭代器是否为数字$(isInteger)$，如果是返回$true$，否则就是列表，将栈顶首迭代器指针指向下一位，并栈顶元素的列表入栈，继续迭代到找到一个数字或者栈为空为止
   - $next:$因为在调用$next$之前必定会调用$hasNext$，所以我们可以直接返回栈顶元素，并将迭代器指向下一位

### 复杂度
- 时间复杂度:
  > $O(1)$，$next$为$O(1)$，$hasNext$均摊为$O(1)$
- 空间复杂度:
  > $O(n)$，最坏情况下，嵌套整形列表变为一条链，复杂度为$O(n)$

### Code
```C++ []
class NestedIterator {
public:
	NestedIterator(vector<NestedInteger>& nestedList) {
		m_st.emplace(nestedList.begin(), nestedList.end());
	}

	int next() {
		return m_st.top().first++->getInteger();
	}

	bool hasNext() {
		while (!m_st.empty()) {
			auto& [begin, end] = m_st.top();
			if (begin == end) {
				m_st.pop();
				continue;
			}

			if (begin->isInteger()) return true;
			auto& child_list = begin++->getList();
			m_st.emplace(child_list.begin(), child_list.end());
		}
        return false;
	}
private:
	stack<pair<vector<NestedInteger>::iterator, vector<NestedInteger>::iterator>> m_st;
};
```