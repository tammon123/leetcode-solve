# 哈希表+二分查找
## 方法一(哈希表+二分查找)：
### 思路
1. 键对应多个值，并且每个值都是不同的$timestamp$，那么我们可以哈希表存储键值对`pair<int, string>`。

2. 获取键对应值时，由于`set`操作中的时间戳$timestamp$都是严格递增的，那么我们可以通过二分查找大于$timestamp$的值对应的位置然后减一就能获得满足条件$timestamp\_prev <= timestamp$的值。
### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为$set$的操作次数，初始化与$set$操作为$O(1)$，$set$二分查找需要的时间为$O(logn)$
- 空间复杂度:
  > $O(n)$，$n$为$set$的操作次数，哈希表需要存储对应的值

### Code
```C++ []
class TimeMap {
public:
	TimeMap() {

	}

	void set(string key, string value, int timestamp) {
		um[key].emplace_back(timestamp, value);
	}

	string get(string key, int timestamp) {
		auto& r = um[key];
		auto i = upper_bound(r.begin(), r.end(), timestamp, [&](int val, pair<int, string>& p) {return val < p.first;}) - r.begin();
		if (i - 1 >= 0) {
			return r[i - 1].second;
		}
		return "";
	}
private:
	unordered_map<string, vector<pair<int, string>>> um;
};
```
