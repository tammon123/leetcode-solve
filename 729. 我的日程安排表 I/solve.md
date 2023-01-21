# 有序集合
## 方法一(链表)：
### 思路
1. 可以将数据放入有序集合中，那么满足$end_i<=start||end<=start_{i+1}$，那么我们每次$book$，只需要二分找到大于等于$end$的第一数，满足它前一个数的$end<=start$或者是找到第一个元素，就能插入日程表数据中

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为日程表的数量，每次二分需要$O(logn)$时间
- 空间复杂度:
  > $O(n)$，集合存储日程表的数量

### Code
```C++ []
class MyCalendar {
public:
	MyCalendar() {

	}

	bool book(int start, int end) {
		auto it = m_calendars.lower_bound({ end, 0 });
		if (it == m_calendars.begin() || (--it)->second <= start) {
			m_calendars.emplace(start, end);
			return true;
		}
		return false;
	}
private:
	set<pair<int, int>> m_calendars;
};
```