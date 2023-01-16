# 递归+回溯
## 方法一(递归+回溯)：
### 思路
1. 全排列，我们可以假设每个位置都需要数组中的一个数填写，我们用数组$visit$标记用过的数组中数的下标。采用递归加回溯的方法列出所有解。
2. 但是因为数组中有重复元素，可能排列会有重复情况，例如：
```
nums: [1,1,2]
按顺序用为: [1,1,2]
第一位用数组第二位的1，第二位用数组第一位的1: [1,1,2]
重复了
```
3. 所以我们可以考虑对数组先进行排序，那么重复元素会相邻，那么我们在递归过程中，判断只要满足```visit[i]||i>0&&nums[i]==nums[i-1]&&!visit[i-1]```，就不遍历下去，满足这个条件，说明当前位置已经有重复元素选用过了，在所有排列中不需要再次出现

### 复杂度
- 时间复杂度:
  > $O(n*n!)$，$n$为序列$nums$的长度，全排有$n!$种
- 空间复杂度:
  > $O(n)$，标记数组$visit$需要$n$个元素大小，递归栈深度会到达$O(n)$

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<vector<int>> ans;
        vector<int> visit(n);
        vector<int> temp;
        auto dfs = [&](int cur, auto&& dfs) {
            if (cur == n) {
                ans.emplace_back(temp);
                return;
            }
            for (int i = 0;i < n;++i) {
                if (visit[i] || i > 0 && nums[i] == nums[i - 1] && !visit[i - 1]) continue;
                temp.emplace_back(nums[i]);
                visit[i] = 1;
                dfs(cur + 1, dfs);
                visit[i] = 0;
                temp.pop_back();
            }
        };
        dfs(0, dfs);
        return ans;
    }
};
```