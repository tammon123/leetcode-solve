# 二分查找+双指针判断子序列
## 方法一(二分查找+双指针判断子序列)：
### 思路
1. 由题目知道，如果当前$k$为最大的值使得$p$为$s$的子序列。那么删除$k$之前的都能让$p$是$s$的子序列成立，查找$k$过程满足单调性，所以可以用二分查找来查找$k$。

2. 判断$p$是否为$s$的子序列，可用双指针方法。用双指针$i,j$分别指向$s,p$开头。如果$i$不在删除列表中，并且$s[i]=p[j]$，那么后移$j$指针，迭代完$s,p$，最终$j=p.sise()$说明$p$是$s$的子序列，否则不是。
### 复杂度
- 时间复杂度:
  > $O((m+n)*logl)$，$m,n$为字符串$s,p$的大小，$l$为数组$removable$大小。二分需要$O(logl)$时间，每次二分需要$O(m+n)$时间，所以最终需要$O((m+n)*logl)$
- 空间复杂度:
  > $O(m)$，二分数组$check$需要的大小

### Code
```C++ []
class Solution {
public:
    int maximumRemovals(string s, string p, vector<int>& removable) {
        int m = s.size(), n = p.size(), left = 0, right = removable.size(), ans = 0;
        int check[m + 1];
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            memset(check, 0, sizeof check);
            for (int i = 0;i < mid;++i) check[removable[i]] = 1;

            int i = 0, j = 0;
            while (i < m && j < n) {
                if (!check[i] && s[i] == p[j]) {
                    ++j;
                }
                ++i;
            }
            if (j == n) {
                ans = mid;
                left = mid + 1;
            }
            else right = mid - 1;
        }
        return ans;
    }
};
```