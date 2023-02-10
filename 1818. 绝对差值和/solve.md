# 二分查找
## 方法一(二分查找)：
### 思路
1. 因为最多替换一个位置，其实是在$nums1$中找到离当前点$nums2[i]$最近的点，所以我们可以用个替换数组$sortArr=nums1$，然后排序，然后用二分查找最近的点，因为是绝对值，查找还需要注意往后的一点。

2. 替换后总和是要变小，求变化后绝对差值和最小，那么就需要前后差值最大。假设当前点为$[nums1[i],nums2[i]]$，找到新的点为$nums1[j],nums2[i]$，那么有: $|nums1[i]-nums2[i]|-|nums1[j]-nums2[i]|$，这个值需要最大。

3. 过程我们用$sum$计算差值和，然后用变量$maxdiff$计算改变后的最大差值
   

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums1,nums2$的大小，我们需要遍历数组$O(n)$，然后每次需要二分找到新值$O(logn)$，排序需要$O(nlogn)$，最终为$O(nlogn)$
- 空间复杂度:
  > $O(n)$，排序需要空间$O(logn)$，排序数组需要$O(n)$

### Code
```C++ []
class Solution {
public:
    static const int MOD = 1e9 + 7;
    int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) {
        auto sortArr = nums1;
        sort(sortArr.begin(), sortArr.end());
        int n = nums1.size();

        int sum = 0, maxdiff = 0;
        for (int i = 0;i < n;++i) {
            int x = nums1[i], y = nums2[i];
            int diff = abs(x - y);
            sum = (long)(sum + diff) % MOD;
            int j = lower_bound(sortArr.begin(), sortArr.end(), y) - sortArr.begin();
            if (j < n) {
                maxdiff = max(maxdiff, diff - (sortArr[j] - y));
            }
            if (j > 0) {
                maxdiff = max(maxdiff, diff - (y - sortArr[j - 1]));
            }
        }

        return (sum - maxdiff + MOD) % MOD;
    }
};
```