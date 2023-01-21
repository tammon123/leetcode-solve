# 变长数组+哈希表
## 方法一(栈)：
### 思路
1. 插入删除，哈希表都可以做到$O(1)$，但是无法根据下标随机定位到元素，不能再$O(1)$时间内完成，而变长数组可以。我们可以用变长数组存值，哈希表键值存值，值存对应变长数组值所在的下标
   - ```RandomizedSet()```:初始化随机种子
   - ```bool insert(int val)```: 先哈希表判断是否有值，有返回$false$，没有就在数组末尾插入值，而下标就是长度减一。再哈希表中存入键值与下标
   - ```bool remove(int val)```: 先哈希表中判断，没有返回$false$。有值，就可以在哈希表找到$val$下标$index$，从数组中取出末尾值，替换$index$位置的值，然后弹出末尾元素。然后将哈希表对应数组末尾值的下标修改为$index$，并删除$val$的值
   - ```int getRandom()```: 返回数组随机值

### 复杂度
- 时间复杂度:
  > $O(1)$，各项都是$O(1)$操作
- 空间复杂度:
  > $O(n)$，输入的元素个数，数组与哈希表都需要$O(n)$空间

### Code
```C++ []
class RandomizedSet {
public:
	RandomizedSet() {
		srand(time(NULL));
	}

	bool insert(int val) {
		if (m_indices.count(val)) return false;
		int index = m_vals.size();
		m_vals.emplace_back(val);
		m_indices[val] = index;
		return true;
	}

	bool remove(int val) {
		if (!m_indices.count(val)) return false;
		int index = m_indices[val];
		int back = m_vals.back();
		m_vals[index] = back;
		m_vals.pop_back();
		m_indices[back] = index;
		m_indices.erase(val);
        return true;
	}

	int getRandom() {
		int index = rand() % m_vals.size();
		return m_vals[index];
	}
private:
	vector<int> m_vals;
	unordered_map<int, int> m_indices;
};
```