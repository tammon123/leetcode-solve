# 递归
## 方法一（递归）：
### 思路
1. 先序遍历的特点，先序遍历数组中的首个数字一定是根节点。那么我们可以在中序数组中找到这个数字，根据中序遍历的特点，那么这个数字左边的构成树的左子树，右边构成右子树。可以把这个数字前的数量统计出来，就是左子树的数量，然后在先序数组中从刚才的根节点往后左子树个数量范围中在递归找左子树根节点。右子树同理可得，那么完整的树就在递归中可以构建好了。
2. 那么我们可以先预处理中序数组数字对应的下标值，用哈希表映射。
3. 递归结束条件：那么先序区间左值大于区间右值返回$null$，其他返回根节点$root$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为节点数
- 空间复杂度:
  > $O(n)$，哈希表存储的节点数，其中O(h)为递归所需空间，即树的高度，这里h<n

### Code
```C++ []
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int, int> um;
        int n = inorder.size();
        for (int i = 0;i < n;++i) um[inorder[i]] = i;

        auto dfs = [&](int preorder_left, int preorder_right, int inorder_left, int inorder_right, auto&& dfs) -> TreeNode* {
            if (preorder_left > preorder_right) return nullptr;

            int preorder_root_index = preorder_left;
            int inorder_root_index = um[preorder[preorder_root_index]];
            int left_cnt = inorder_root_index - inorder_left;
            TreeNode* root = new TreeNode(preorder[preorder_root_index]);
            root->left = dfs(preorder_left + 1, preorder_left + left_cnt, inorder_left, inorder_root_index - 1, dfs);
            root->right = dfs(preorder_left + left_cnt + 1, preorder_right, inorder_root_index + 1, inorder_right, dfs);
            return root;
        };
        return dfs(0, n - 1, 0, n - 1, dfs);
    }
};
```