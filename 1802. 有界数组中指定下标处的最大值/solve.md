# 贪心+二分查找
## 方法一(贪心+二分查找)：
### 思路
1. 由于需要指定下标处的值最大，使得最终的和小于等于$target$。又由于相邻两个数不超过$1$，且所有数都是正整数，所以根据贪心的思想，我们需要从指定下标处向左右每次都递减$1$，如果还没到结束先到$1$了，最后补充$1$。

2. 由于值越大，和越大，我们可以通过二分的方法查找指定下标的值。如果和小于等于$target$，说明还能更小，收敛左边界，否则收敛右边界。搜索范围只能是$[1,target]$。

3. 如果直接向两边遍历递减，由于数据量很大，肯定会超时。我们考虑下标处左边的长度$left_{len}=index$，右边的长度$right_{len}=n-index+1$。我们考虑左边，右边同理:
   - 如果$left_{len}<mid-1$，说明不用补充$1$，那么根据求和公式有$\sum(\frac{left_{len}*(mid-left_{len})}{2})$
   - 如果$left_{len}>=mid-1$，说明需要补充$ones=left_{len}-mid+1$个$1$，最终和为:$\sum(\frac{mid*(mid-1)}{2})+ones$
### 复杂度
- 时间复杂度:
  > $O(log(maxSum))$，$maxSum$为给定值，二分需要$O(log(maxSum))$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maxValue(int n, int index, int maxSum) {
        int left = 1, right = maxSum, left_len = index, right_len = n - index - 1;

        auto getsum = [&](int len, int mid)->long {
            if (len + 1 < mid) {
                int low = mid - len;
                return (long)len * (mid - 1 + low) / 2;
            }
            else {
                int ones = len - mid + 1;
                return (long)mid * (mid - 1) / 2 + ones;
            }
        };
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            long sum = getsum(left_len, mid) + getsum(right_len, mid) + mid;
            if (sum <= maxSum) {
                left = mid + 1;
            }
            else right = mid - 1;
        }
        return right;
    }
};
```