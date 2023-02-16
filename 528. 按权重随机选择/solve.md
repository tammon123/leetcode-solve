# 前缀和+二分查找
## 方法一(前缀和+二分查找)
### 思路
1. 假设数组总和$total$，我们按照权重随机选择，那么就是把$[1,total]$分成$n$组，然后随机化$x∈[1,total]$，找到$x$属于哪个组。

2. 例如：$[1,2,3,4]$，可以分为$[1,1],[2,3],[4,5,6],[7,8,9,10]$，我们可以看到，划分组的右区间是前缀和值，左区间是上一个区间值加一。那么我们可以求出前缀和，因为前缀和单调递增，所以我们可以通过二分的方法，使得$prefixsum[i]-w[i]+1<=x<=prefixsum[i]$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$w$的大小，初始化需要$O(n)$，每次选择需要$O(logn)$
- 空间复杂度:
  > $O(n)$，前缀和需要的空间

### Code
```C++ []
class Solution {
public:
	Solution(vector<int>& w): uid(1, accumulate(w.begin(), w.end(), 0)) {
		partial_sum(w.begin(), w.end(), back_inserter(prefixsum));
	}

	int pickIndex() {
		int x = uid(mt);
		return lower_bound(prefixsum.begin(), prefixsum.end(), x) - prefixsum.begin();
	}
private:
	mt19937 mt;
	uniform_int_distribution<int> uid;
	vector<int> prefixsum;
};
```