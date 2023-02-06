# 自定义排序/二分查找+双指针
## 方法一(自定义排序)：
### 思路
1. 我们根据条件```|a - x| < |b - x| 或者 |a - x| == |b - x| 且 a < b```，可以将数组按照条件排序，然后取前$k$个数后按照从小到达的顺序排序作为答案

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，主要是排序的操作$O(nlogn)$
- 空间复杂度:
  > $O(logn)$，排序需要的栈空间

### Code
```C++ []
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        sort(arr.begin(), arr.end(), [&](int a, int b) -> bool {
            return abs(a - x) < abs(b - x) || (abs(a - x) == abs(b - x) && a < b);
        });
        sort(arr.begin(), arr.begin() + k);
        return vector<int>(arr.begin(), arr.begin() + k);
    }
};
```

## 方法二(二分查找+双指针)：
### 思路
1. 因为数组是非递减的，我们可以用二分找到大于等于$x$的第一个数作为有右指针$right$，然后左指针$left=right-1$

2. 那么我们只需要移动左右指针来扩大到$k$个范围：
   - 如果$left<0$，我们让$right+1$，剩余的数都在右边
   - 如果$right>=n$，我们让$left-1$，剩余的数都在左边
   - 如果$x-arr[left]<=arr[right]-x$，说明满足条件，我们选择小的$left$对应值，让$left-1$
   - 其他的让$right+1$

	最终范围为$[left+1,right-1]$

### 复杂度
- 时间复杂度:
  > $O(logn+k)$，二分需要$O(logn)$，需要$O(k)$迭代范围
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int right = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        int left = right - 1, n = arr.size();
        while (k--) {
            if (left < 0) ++right;
            else if (right >= n) --left;
            else if (x - arr[left] <= arr[right] - x) --left;
            else ++right;
        }
        return vector<int>(arr.begin() + left + 1, arr.begin() + right);
    }
};
```