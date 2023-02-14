# 排序+滑动窗口
## 思路
1. 假设范围$[i,j]$的元素需要转化为到数$x$，范围内最大的值为$y$，如果$x>y$，我们将范围内的所有数转化为$y$的频率跟转化为$x$频率一样且操作数更少，所以转化肯定往数组中的数转化更好。

2. 如果有两个数$1,4$需要转化到$5$，只能转化一个，那么优先转化差值最小的。

3. 根据这两点，我们可以用贪心思想，把数组排序后，遍历数组，以每个点作为右窗口来滑动左窗口。直接双重遍历肯定超时，我们需要考虑，当前右窗口为$r$，左窗口为$l$，那么$[l,r]$的数都转化到$r$需要的操作数为:$\sum\limits_{i=l}(nums[r]-nums[i])$。
   
   那么右窗口滑动，操作数变化为：
   
   $\Delta=\sum\limits_{i=l}(nums[r+1]-nums[i])-\sum\limits_{i=l}(nums[r]-nums[i])=(nums[r+1]-nums[r])*(r-l)>=0$
   
   向右滑动操作数增加的，有可能超过$k$，需要左窗口滑动

   左窗口滑动，操作数变化为：

   $\Delta=-(nums[r+1]-nums[l])<=0$

   向左滑动操作数减少，直到新的$l'$，使得操作数重新$<=k$

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，其中$n$为数组$nums$的大小，排序需要$O(nlogn)$，滑窗需要$O(n)$
- 空间复杂度:
  > $O(logn)$，排序需要空间大小

### Code
```C++ []
class Solution {
public:
    int maxFrequency(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        long long total = 0ll;
        int j = 0, ans = 1;
        for (int i = 1;i < nums.size();++i) {
            total += (long long)(nums[i] - nums[i - 1]) * (i - j);
            while (total > k) {
                total -= nums[i] - nums[j++];
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```