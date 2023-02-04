# 一次遍历
## 方法一(一次遍历)：
### 思路
1. 由题目知，我们每次操作为: ```nums1[i] = nums1[i] + k``` 且 ```nums1[j] = nums1[j] - k``` 。那么要使$nums1$转化为$nums2$最小操作数，我们可以针对每个$i$，对应的$nums1[i]$转为$nums2[i]$是加$mk$还是减$mk$，$m$表示倍数，如果不是倍数，说明不能转化。

2. 那么我们只需要计算增加操作数与减少的操作数是否相等就行。

3. 需要注意如果$k=0$，那么$nums1=nums2$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums1$与$nums2$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    long long minOperations(vector<int>& nums1, vector<int>& nums2, int k) {
        long long neg = 0ll, pos = 0ll;
        int n = nums1.size();
        for (int i = 0;i < n;++i) {
            int diff = nums1[i] - nums2[i];
            if (k == 0 && diff != 0) return -1;
            else if( k == 0) continue;
            if (diff % k != 0) return -1;
            long long op = diff / k;
            op > 0 ? pos += op : neg -= op;
        }
        return pos == neg ? pos : -1;
    }
};
```
