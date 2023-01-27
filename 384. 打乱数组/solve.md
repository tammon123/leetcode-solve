# 随机化+删除数据操作
## 方法一(随机化+删除数据操作)：
### 思路
1. 因要所有排列均等，那么我们只要把数组每个元素都从原数组随机排列。
    - 例如：数组为$[1,2,3]$，我们首先从三个数字随机取一个作为第一个元素，假如随机到$3$
    - 接着我们从剩余元素随机取，假如随机到$1$，最后为$[3,1,2]$

2. 那么我们要怎么选择剩余数据呢，可以参照删除操作。把要删除数据与末尾元素交换，然后删除末尾元素，就可以做到$O(1)$操作。我们不需要删除数据，也不需要交换队尾，反过来从队头交换。那么我们从左到右遍历数据，假设当前遍历到$i$，之前都随机化完毕。当前需要进行交换操作:```swap(nums[i], rand()%(n-i)+i)```

3. 那么在初始化的时候，可以设置一个随机种子，并记录初始数组与要随机化操作的数组

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$数组$nums$的大小。
  - 初始化:$O(n)$
  - $reset()$:$O(1)$
  - $shuffle()$: $O(n)$
- 空间复杂度:
  > $O(n)$，记录初始数组与随机化数组需要$O(n)$大小

### Code
```C++ []
class Solution {
public:
	Solution(vector<int>& nums):m_nums(nums), shuffle_nums(nums) {
		srand(time(nullptr));
		n = nums.size();
	}

	vector<int> reset() {
		return m_nums;
	}

	vector<int> shuffle() {
		for (int i = 0;i < n;++i) {
			int index = rand() % (n - i) + i;
			swap(shuffle_nums[i], shuffle_nums[index]);
		}
		return shuffle_nums;
	}
private:
	vector<int> m_nums;
	vector<int> shuffle_nums;
	int n;
};
```