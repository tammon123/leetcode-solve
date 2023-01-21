# 动态规划
## 方法一(动态规划)：
### 思路
1. 每一个房间可以选择偷或者不偷，假设当前房间偷为$rob[i]$，不偷为$no\_rob[i]$，那么这次偷的应该由上次不偷加这次偷，或者上次偷这次不偷的最大值转移来有：$rob[i]=max(rob[i-1],no\_rob[i-1]+nums[i])$。这次不偷最大值由上次偷转移来有$no\_rob[i]=rob[i-1]$，最终为能偷最大值为$max(rob[n-1],no_rob[n-1])$的结果，这是房屋不成环的结果。
2. 现在首尾相连，第一个房屋偷了，那么最后一个房屋就不能偷。那么我们需要考虑第一个房屋偷，到$n-1$个房屋的情况。以及第一个房屋不偷，到$n$个房屋的情况。即$max([0,n-1],[1,n])$
3. 过程可用状压，因为当前状态只跟上一次状态有关，可用两个变量分别来表示偷与不偷

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$大小，遍历两次数组
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int rob(vector<int>& nums) {
        auto check = [&](int start, int end)->int {
            int rob = nums[start], no_rob = 0;
            for (int i = start + 1;i < end;++i) {
                tie(rob, no_rob) = make_pair(max(no_rob + nums[i], rob), rob);
            }
            return max(rob, no_rob);
        };

        int n = nums.size();
        return max(check(0, n - 1), n == 1 ? 0 : check(1, n));
    }
};
```