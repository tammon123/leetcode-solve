# 递归+回溯
## 方法一(递归+回溯)：
### 思路
1. 组合可用递归加回溯思想。递归到当前元素选择或者不选择，然后进行回溯，如果选择当前元素，我们$target$需要减去当前元素，继续递归。但是本题重复元素可以多次选择，那么我们在递归的时候需要以当前元素传递，不选择再往后递归。
2. 我们还可以剪枝递归过程，我们可优先把数组进行从小到大排序，如果$target$小于当前元素，说明后面元素都不需要再进行比较，直接跳过

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$candidates$的长度，排序需要$O(nlogn)$，因为组合上界为$O(n*2^n)$，但是剪枝后最终会小于$O(nlogn)$
- 空间复杂度:
  > $O(target)$，递归栈深度最差为$O(target)$

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