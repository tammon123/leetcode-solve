# 排序+二分查找/排序+双指针
## 方法一(排序+二分查找)：
### 思路
1. 三角形需要满足条件$
   \begin{cases}
  & a+b>c \\
  & a+c>b \\
  & b+c>a
\end{cases}
   $，那么我们首先可以把数组进行排序，使得$a<b<c$，那么必有$a+c>b，b+c>a$，所以我们只需判断$a+b>c$的条件。

2. 假设我们固定了两点$i<j$，我们只需要找到最小的点$k$，使得$nums[k]>=nums[i]+nums[j]$，因为排序后数组是递增的，那么我们 可以用二分查找范围再$[j+1,n-1]$的点$k$，然后有$[j+1,k-1]$都是满足三角形条件的$k$值，答案加上$k-j-1$

### 复杂度
- 时间复杂度:
  > $O(n^2logn)$，$n$为数组$nums$的大小，排序需要$O(nlogn)$，二分查找需要$O(logn)$，再$i,j$双重循环下，所以最终趋于$O(n^2logn)$
- 空间复杂度:
  > $O(logn)$，排序需要的栈空间

### Code
```C++ []
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int n = nums.size(), ans = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0;i < n - 2;++i) {
            for (int j = i + 1;j < n - 1;++j) {
                int x = nums[i] + nums[j];
                int k = lower_bound(nums.begin() + j + 1, nums.end(), x) - nums.begin();
                ans += k - j - 1;
            }
        }
        return ans;
    }
};
```

## 方法二(排序+双指针)：
### 思路
1. 跟方法一一样，我们先排序后，只需要判断一个条件$a+b>c$。假设当前$i<j<k$，满足了$nums[i]+nums[j]<=nums[k]$，有$[j+1,k-1]$范围的值满足条件了。那么双重循环$i$不动的情况下，$nums[j]$是往右递增的，即${j}'>j$，那么要重新满足条件的$nums[k]$肯定也要增加，于是再$k$的基础上继续往右找到新的${k}'$，重新满足$nums[i]+nums[{j}']<=nums[{k}']$

2. 注意$n>{k}'>=max(k,{j}')>j>i$，即$k=min(n-1,max(k,j+1))$，每次加上的答案长度为$k-j-1$

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为数组$nums$的大小，排序需要$O(nlogn)$，$j,k$最终需要$O(n)$，所以趋向于双重遍历$O(n^2)$
- 空间复杂度:
  > $O(logn)$，排序需要的栈空间
### Code
```C++ []
class Solution {
public:
    int triangleNumber(vector<int>& nums) {
        int n = nums.size(), ans = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0;i < n - 2;++i) {
            int k = i + 2;
            for (int j = i + 1;j < n - 1;++j) {
                int x = nums[i] + nums[j];
                k = min(n - 1, max(k, j + 1));
                while (k < n && nums[k] < x) {
                    ++k;
                }
                ans += k - j - 1;
            }
        }
        return ans;
    }
};
```