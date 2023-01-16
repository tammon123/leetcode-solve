# 原地模拟
## 方法一(原地模拟)：
### 思路
1. 每次操作后减少一半数，那么我们可以原地模拟

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的长度，每次迭代需要遍历数组减半，渐进为$O(n)$
- 空间复杂度:
  > $O(1)$，原地模拟所以为$O(1)$

### Code
```C++ []
class Solution {
public:
    int minMaxGame(vector<int>& nums) {
        int n = nums.size();
        while (n != 1) {
            int m = n >> 1;
            for (int i = 0;i < m;++i) {
                nums[i] = i & 1 ? max(nums[i << 1], nums[(i << 1) + 1]) : min(nums[i << 1], nums[(i << 1) + 1]);
            }
            n = m;
        }
        return nums[0];
    }
};
```