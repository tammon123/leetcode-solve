# 深度优先搜索
## 思路
1. 通过深搜遍历树，假设递归到当前节点为$cur$
   - 如果$cur<key$，说明$key$在右子树，递归右子树
   - 如果$cur>key$，说明$key$在左子树，递归左子树
   - 如果$cur=key$，根据搜索树特性，那么当前节点$cur$的右子树的最左一定大于当前节点的左子树的根节点，我们只需要不断迭代右子树的左子树，到达叶子节点，然后把当前节点的左子树根节点指向这个叶子节点的左端就行，最终返回右子树根节点。需要注意右子树如果为空的情况，我们直接返回当前节点的左子树。
## 复杂度
- 时间复杂度:
  > $O(n)$，n节点数
- 空间复杂度:
  > $O(n)$，n取决于栈空间开销

## Code
```C++ []
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        auto dfs = [&](TreeNode* node, auto&& dfs)->TreeNode* {
            if (!node) return nullptr;

            if (key == node->val) {
                TreeNode* left = node->left;
                TreeNode* right = node->right;
                delete node;
                TreeNode* cur = right;
                while (cur && cur->left) {
                    cur = cur->left;
                }
                if (cur) {
                    cur->left = left;
                    return right;
                }
                else return left;
            }
            else if (key < node->val) node->left = dfs(node->left, dfs);
            else node->right = dfs(node->right, dfs);
            return node;
        };
        return dfs(root, dfs);
    }
};
```