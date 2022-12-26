# 深度优先搜索（回溯）
## 思路
1. 通过深搜遍历树，对每个节点求和并且把当前节点存入缓存数组中，最终到叶子节点和等于目标数，则把缓存数组的存入结果。
2. 遍历完左右子树，弹出缓存数组中的最后一个值$(back)$，整个遍历采用回溯的思想。
## 复杂度
- 时间复杂度:
  > $O(n)$，n节点数
- 空间复杂度:
  > $O(n)$，n取决于栈空间开销

## Code
```C++ []
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> ans;
        vector<int> temp;
        auto dfs = [&](TreeNode* node, int sum, auto&& dfs) ->void {
            if (!node) return;

            temp.emplace_back(node->val);
            sum += node->val;
            if (!node->left && !node->right && sum == targetSum) {
                ans.emplace_back(temp);
            }
            dfs(node->left, sum, dfs);
            dfs(node->right, sum, dfs);
            temp.pop_back();
        };
        dfs(root, 0, dfs);
        return ans;
    }
};
```