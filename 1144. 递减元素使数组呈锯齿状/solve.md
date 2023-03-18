# 贪心+分类讨论
## 思路
1. 因为每次操作只会减少$1$，且所有偶数索引大于奇数索引或者所有奇数索引大于偶数索引。那么我们只需要分类讨论两种情况最终的操作数。

2. 假如当前讨论偶数索引的操作和，那么所有奇数索引数需要减少到两边偶数位置最小值加一，操作数有:$nums[i]-min(nums[i-1],nums[i+1])+1$，注意操作数小于$0$，需要改为$0$。同理奇数索引操作和一样。

3. 最后比较两个和的大小

### 复杂度
- 时间复杂度:
  > $O(n)$，最终遍历了整个数组$O(n)$
- 空间复杂度:
  > $O(1)$，若干变量

### Code
```C++ []
class Solution {
public:
    int movesToMakeZigzag(vector<int>& nums) {
        int ans[2]{}, n = nums.size();
        for (int i = 0;i < 2;++i) {
            for (int j = i;j < n;j += 2) {
                int a = j ? nums[j - 1] : INT_MAX;
                int b = j < n - 1 ? nums[j + 1] : INT_MAX;
                ans[i] += max(nums[j] - min(a, b) + 1, 0);
            }
        }
        return min(ans[0], ans[1]);
    }
};
```