# 贪心
## 方法一(贪心)：
### 思路
1. 当前最大能到达的位置为$maxPos=max(maxPos, nums[i]+i)$，在最大位置内都能随便到达，我们从左往右依次更新最大位置，当遍历到的位置$maxPos>=n-1$说明我们能到最后一个位置，不然当前位置$i$已经是最大位置了，说明不能再往前走了，即$i==maxPos$返回$false$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int maxPos = 0, n = nums.size();
        for (int i = 0;i < n;++i) {
            maxPos = max(maxPos, nums[i] + i);
            if (maxPos >= n - 1) return true;
            else if (maxPos == i) return false;
        }
        return false;
    }
};
```