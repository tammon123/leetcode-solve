# 哈希表+二分查找
## 方法一(哈希表+二分查找)：
### 思路
1. 如果按照快照来存储数据，每次存储数据量大，我们可以快照存储变化数据。

2. 用哈希表存储$index, snap\_id, val$，我们可以直接存有序映射，然后通过二分找到$snap\_id$对应的值。

### 复杂度
- 时间复杂度:
  
  - set: 内部$O(logn)$
  - snap: $O(1)$
  - get: 二分需要$O(logn)$
- 空间复杂度:
  > $O(n)$，哈希表需要存储变化的所有数据

### Code
```C++ []
class SnapshotArray {
public:
	SnapshotArray(int length):snapCnt(0) {
	}

	void set(int index, int val) {
		um[index][snapCnt] = val;
	}

	int snap() {
		return snapCnt++;
	}

	int get(int index, int snap_id) {
		auto& snapVals = um[index];
		auto it = snapVals.upper_bound(snap_id);
		if (it != snapVals.begin()) {
			return (--it)->second;
		}
        return 0;
	}
private:
	unordered_map<int, map<int, int>> um;
	int snapCnt;
};
```