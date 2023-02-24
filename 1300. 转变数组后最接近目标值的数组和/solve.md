# 前缀和+二分查找
## 方法一(前缀和+二分查找)：
### 思路
1. 我们要找到$value$，使得数组中大于$value$的所有值转化为$value$，最终转化后的数组和要接近$target$。我们可以知道，$value$值取值范围为$[1,max(arr)]$，因为数组中所有值都为正整数，如果$value<=0$，所有值的和不会变与$value=1$最终结果是一样的，所以$value>=1$。如果$value>max(arr)$最终与$value=max(arr)$数组的和结果也是一样的，所以$value<=max(arr)$。

2. 由于$value$值越大，和也会越大，我们可以通过二分查找$value$。如果总和$sum>=target$，说明和还可以更小，那么$value$也可以更小，收敛右边界，否则收敛左边界。我们最终需要计算左值与右值和，取其中最靠近$target$的值。

3. 对于求和，大于$value$的都需要变为$value$，可以对数组先进行排序，然后求出前缀和，通过二分查找$arr$定位到前缀和的位置，在$O(1)$时间内可以求出前缀和。最终和$sum=prefixsum[i]+value*(n-i)$。
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$arr$的大小，排序需要$O(nlogn)$，求前缀和需要$O(n)$，二分范围需要$O(log(max(arr)))$，二分前缀和需要$O(logn)$，求左右和的值需要遍历数组$O(n)$，最终趋于$O(nlogn)$
- 空间复杂度:
  > $O(n)$，需要$O(n)$来存储前缀和，排序需要$O(logn)$栈空间，最终趋于$O(n)$

### Code
```C++ []
class Solution {
public:
    int findBestValue(vector<int>& arr, int target) {
        sort(arr.begin(), arr.end());
        int n = arr.size();
        vector<int> prefixsum(n + 1);
        for (int i = 1;i <= n;++i) {
            prefixsum[i] = prefixsum[i - 1] + arr[i - 1];
        }
        int left = 1, right = *max_element(arr.begin(), arr.end());
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            int i = lower_bound(arr.begin(), arr.end(), mid) - arr.begin();
            long long sum = prefixsum[i] + mid * (n - i);
            if (sum >= target) {
                right = mid - 1;
            }
            else {
                left = mid + 1;
            }
        }
        int a1 = 0, a2 = 0;
        for (auto&& x : arr) {
            if (x > left) a1 += left;
            else a1 += x;
        }
        for (auto&& x : arr) {
            if (x > right) a2 += right;
            else a2 += x;
        }
        return abs(a1 - target) >= abs(a2 - target) ? right : left;
    }
};
```