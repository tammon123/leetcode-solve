# 模拟/二分查找
## 方法一(模拟)：
### 思路
1. 直接计算正整数与负整数的数量，然后比较大小

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int pos = 0, neg = 0;
        for (auto&& num : nums) {
            if (num > 0) ++pos;
            else if (num < 0) ++neg;
        }
        return max(pos, neg);
    }
};
```

## 方法二(二分查找)：
### 思路
1. 因为数据满足单调性，我们可以找到$0$的所在的区间$[l,r]$，表示有$l$个负整数，有$n-r$个正整数，比较两个数的大小

### 复杂度
- 时间复杂度:
  > $O(logn)$，$n$为数组$nums$的大小，二分查找需要$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int n = nums.size();
        int l = lower_bound(nums.begin(), nums.end(), 0) - nums.begin();
        int r = upper_bound(nums.begin(), nums.end(), 0) - nums.begin();

        return max(l, n - r);
    }
};
```