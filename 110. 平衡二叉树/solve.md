# 深度优先搜索
## 方法一（深度优先搜索）：
### 思路
1. 通过自底向上的深搜，判断左子树跟右子树的高度，如果大于$1$，返回$-1$，不然返回左右子树最大高度加$1$，空节点返回$0$，最终结果判断是否大于等于$0$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为树的节点数
- 空间复杂度:
  > $O(n)$，n为递归调用层数

### Code
```C++ []
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        auto dfs = [&](TreeNode* node, auto&& dfs)->int {
            if (!node) return 0;
            int left = dfs(node->left, dfs);
            int right = dfs(node->right, dfs);
            if (left == -1 || right == -1 || abs(left - right) > 1) return -1;
            return max(left, right) + 1;
        };
        return dfs(root, dfs) >= 0;
    }
};
```
