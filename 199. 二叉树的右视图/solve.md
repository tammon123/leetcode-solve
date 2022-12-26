# 广度优先搜索
## 思路
1. 层次遍历并且扫描右边的第一个节点，那么我们在层次遍历节点入队的时候，可以先让右子树入队，保证每次扫描都是取右边第一个扫描。
## 复杂度
- 时间复杂度:
  > $O(n)$，n为节点数
- 空间复杂度:
  > $O(n)$

## Code
```C++ []
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int> ans;
        if (!root) return ans;

        queue<TreeNode*> q;
        q.emplace(root);
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0;i < size;++i) {
                TreeNode* node = q.front(); q.pop();
                if (i == 0) ans.emplace_back(node->val);
                if (node->right) q.emplace(node->right);
                if (node->left) q.emplace(node->left);
            }
        }
        return ans;
    }
};
```