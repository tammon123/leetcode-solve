# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 由于这棵树是完整二叉树，要么没有叶子节点，要么有两个叶子节点。我们在遍历树的时候只需要判断是叶子节点返回值，否则按照当前的运算计算左右子树的值后再返回值。

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为树$root$的节点数
- 空间复杂度:
  > $O(n)$，完整二叉树深度最大为$O(\frac{n}{2})$，所以递归栈深度为$O(n)$

### Code
```C++ []
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool evaluateTree(TreeNode* root) {
        auto dfs = [&](TreeNode* node, auto&& dfs)->bool {
            if (!node->left && !node->right) return node->val;
            bool left = dfs(node->left, dfs);
            bool right = dfs(node->right, dfs);
            return node->val == 2 ? left || right : left && right;
        };
        return dfs(root, dfs);
    }
};
```
