# 二分查找
## 方法一(二分查找)：
### 思路
1. 我们要求$\prod_{l=j}^{i}nums[l]<k$，那么我们可以两边取对数有：$\sum_{l=j}^{i}\log nums[l]<\log k$，这样我们就能预先算出对数前缀和$prefixlog$
2. 因为$nums$数组是正整数，所以对数后数组是非递减的，所以我们找区间和满足$prefixlog[i+1]-prefixlog[j]<\log k$，即二分找到下标最小$j$满足$prefixlog[j]>prefixlog[i+1]-\log k$，以$i$为右端点的子数组个数为$i-j+1$
3. 因为$double$存在误差，又因为$nums[i]$不会超过$5$位，我们只需要再不等式右边加上$10^{-10}$，防止相等判断为大于情况
### 复杂度
- 时间复杂度:
  > $O(n+nlogn)$，n为数组nums的大小，预处理前缀和需要$O(n)$，枚举二分需要$O(nlogn)$
- 空间复杂度:
  > $O(n)$，存储数组前缀和

### Code
```C++ []
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int n = nums.size(), ans = 0;
        vector<double> prefixlog(n + 1);
        for (int i = 0;i < n;++i) {
            prefixlog[i + 1] = prefixlog[i] + log(nums[i]);
        }
        double logk = log(k);
        for (int i = 0;i < n;++i) {
            int j = upper_bound(prefixlog.begin(), prefixlog.begin() + i + 1, prefixlog[i + 1] - logk + 1e-10) - prefixlog.begin();
            ans += i - j + 1;
        }
        return ans;
    }
};
```
## 方法二(滑动窗口)：
### 思路
1. 假设以$i$为右端点，$j$为左端点的子数组乘积小于$k$，那么子数组个数为$i-j+1$，我们可以从左往右遍历把乘积算出来，然后$j$从$0$开始遍历，有:$\prod_{l=j}^{i}nums[l]<k$，那么以$j$为左端点的左边数只会让积变大，所以当$i+1$时，乘积变大，我们只需要从刚才的$j$位继续往右遍历就行，直到满足新的$\prod_{l=j}^{i}nums[l]<k$，或者$j>i$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组nums的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int n = nums.size(), j = 0, product = 1, ans = 0;
        for (int i = 0;i < n;++i) {
            product *= nums[i];
            while (j <= i && product >= k) {
                product /= nums[j];
                ++j;
            }
            ans += i - j + 1;
        }
        return ans;
    }
};
```