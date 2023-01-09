# 队列+哈希表
## 方法一(队列+哈希表)：
### 思路
1. 不够$k$个存入队列跟哈希表中，返回$false$，超过$k$个值，队列弹出元素，弹出的值对应的哈希表数量减一，如果数量为$0$了，从哈希表中删除
2. 超过$k$后，经由以上操作，判断哈希表大小是否超过$1$，如果超过返回$false$，如果大小为$1$再判断$value$的数量是否为$k$
### 复杂度
- 时间复杂度:
  > $O(1)$，独立操作不计算复杂度
- 空间复杂度:
  > $O(k)$，最多存入$k$个值

### Code
```C++ []
class DataStream {
public:
	DataStream(int value, int k):m_value(value), m_k(k) {
	}

	bool consec(int num) {
		m_q.emplace(num);
		++um[num];
		if (m_q.size() > m_k) {
			int p = m_q.front();
			m_q.pop();
			--um[p];
			if (um[p] == 0) um.erase(p);
		}
		if (um.size() > 1) return false;
		else return um[m_value] == m_k;
	}
private:
	queue<int> m_q;
	unordered_map<int, int> um;
	int m_value;
	int m_k;
};
```