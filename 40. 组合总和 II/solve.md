# 递归+回溯
## 方法一(递归+回溯)：
### 思路
1. 组合可用递归加回溯思想。递归到当前元素选择或者不选择，然后进行回溯，如果选择当前元素，我们$target$需要减去当前元素，继续递归。本题每个元素只能出现一次，那么我们再选择当前元素后，只能往下个元素选择。
2. 我们还可以剪枝递归过程，我们可优先把数组进行从小到大排序，如果$target$小于当前元素，说明后面元素都不需要再进行比较，直接跳过
3. 因为本身数组存在重复元素，我们可能选择不同位置的元素后，但最终排序好的组合结果是一样的，所以我们需要去重思想，可参考[47.全排列Ⅱ](https://leetcode.cn/problems/permutations-ii/)排列数去重思想，所以还是需要把数组进行排序(再步骤2中为了剪枝)，那么我们用个标记数组$visit$，表示当前元素已经选用，只要满足```visit[i]||i>0&&nums[i]==nums[i-1]&&!visit[i-1]```条件，就不再选择当前元素

### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，取最坏情况，有$2^n$中组合，大于排序$O(nlogn)$
- 空间复杂度:
  > $O(n)$，递归栈深度最差为$O(target)$，还需要存储$visit$数组$O(n)$

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        int n = candidates.size();
        sort(candidates.begin(), candidates.end());
        vector<vector<int>> ans;
        vector<int> temp;
        auto dfs = [&](int cur, int need, auto&& dfs) {
            if (need == 0) {
                ans.emplace_back(temp);
                return;
            }
            for (int i = cur;i < n;++i) {
                if (need >= candidates[i]) {
                    temp.emplace_back(candidates[i]);
                    dfs(i, need - candidates[i], dfs);
                    temp.pop_back();
                }
                else break;
            }
        };
        dfs(0, target, dfs);
        return ans;
    }
};
```