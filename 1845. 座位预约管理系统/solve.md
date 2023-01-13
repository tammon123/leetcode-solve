# 最小堆
## 方法一(最小堆)：
### 思路
1. 每次取最小编号，我们可以维护一个最小堆
   - $reserve()$: 我们弹出堆顶元素
   - $unreserve()$: 将$seatNumber$入堆

### 复杂度
- 时间复杂度:
  > $O(n+(k+l)logn)$，开始时候将所有元素入堆$O(n)$，因为堆弹出元素与入堆时间复杂度为$O(logn)$，所以$reserve$与$unreserve$为$k$与$l$次
- 空间复杂度:
  > $O(n)$，堆最多存储$n$个元素

### Code
```C++ []
class SeatManager {
public:
	SeatManager(int n) {
		for (int i = 1;i <= n;++i) {
			m_pq.emplace(i);
		}
	}

	int reserve() {
		int top = m_pq.top();
		m_pq.pop();
		return top;
	}

	void unreserve(int seatNumber) {
		m_pq.emplace(seatNumber);
	}
private:
	priority_queue<int, vector<int>, greater<int>> m_pq;
};
```