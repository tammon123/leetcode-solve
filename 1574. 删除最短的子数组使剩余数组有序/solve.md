# 双指针+二分查找/滑动窗口+二分查找
## 方法一(双指针+二分查找)：
### 思路
1. 首先删除的是子数组，所以只能连续删除，那么我们考虑从，假如分成三部分，从开始到中间是非递减的，中间递减，然后又非递减到结尾。我们用两个指针表示$i,j$，即：$[0,i),[i,j],(j,n-1]$三段。那么我们可以考虑遍历第一段，然后在第三段中二分找到大于等于当前点的值，说明删除中间段能使两段连续非递减。

2. 考虑优化整个过程，我们可以比较一下$[0,i)$与$(j,n-1]$这两段大小，拿长的进行二分。如果是二分$[0,j)$，那么需要找到大于当前值的位置然后减一。
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$arr$大小，我们先线性扫描$i,j$点$O(n)$，假设$[0,i)$长为$n_1$，$(j,n-1]$长为$n_2$，那么线性扫描$O(n1)$，二分$O(logn_2)$，所以需要复杂度为$O(n+n_1logn_2)$，最终趋于$O(nlogn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size(), i = 1, j = n - 1;
        while (i < n && arr[i] >= arr[i - 1]) {
            ++i;
        }
        if (i == n) return 0;
        while (j >= 1 && arr[j - 1] <= arr[j]) {
            --j;
        }
        int ans = n;
        if (i <= n - j) {
            ans = j;
            auto it = arr.begin() + j;
            for (int k = 0;k < i;++k) {
                it = lower_bound(it, arr.end(), arr[k]);
                ans = min(ans, (int)(it - arr.begin()) - k - 1);
            }
        }
        else {
            ans = n - i;
            auto it = arr.begin() + i;
            for (int k = n - 1;k >= j;--k) {
                it = upper_bound(arr.begin(), it, arr[k]);
                ans = min(ans, k - (int)(it - arr.begin()));
            }
        }

        return ans;
    }
};
```

## 方法二(滑动窗口+二分查找)：
### 思路
1. 由方法一知道，我们两段非递减区间都可以互相二分，那么我们考虑假如当前左边遍历到$left$，我们二分找到大于等于$arr[left]$的右边的位置为$right$，假如右边滑动一个值，那么它的值一定大于$arr[left]$，我们在$left$基础上二分，滑动左窗口。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$arr$大小，我们先线性扫描$i,j$点$O(n)$，互相二分$O(log^2n)$，所以需要复杂度为$O(n+log^2n)$，最终趋于$O(n)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int n = arr.size(), i = 1, j = n - 1;
        while (i < n && arr[i] >= arr[i - 1]) {
            ++i;
        }
        if (i == n) return 0;
        while (j >= 1 && arr[j - 1] <= arr[j]) {
            --j;
        }
        int ans = j;
        auto left = arr.begin(), right = arr.begin() + j;
        while (left < arr.begin() + i && right < arr.end()) {
            if (*left <= *right) {
                left = upper_bound(left, arr.begin() + i, *right);
                ans = min(ans, static_cast<int>(right - left));
                ++left;
                ++right;
            }
            else {
                ++right;
                right = lower_bound(right, arr.end(), *left);
            }
        }
        ans = min(ans, n - i);
        return ans;
    }
};
```