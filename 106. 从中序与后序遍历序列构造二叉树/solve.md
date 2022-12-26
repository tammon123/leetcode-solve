# 递归
## 方法一（递归）：
### 思路
1. 参考[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)
2. 后序遍历的特点，后序遍历数组中的最后一个数字一定是根节点。那么我们可以在中序数组中找到这个数字，根据中序遍历的特点，那么这个数字左边的构成树的左子树，右边构成右子树。可以把这个数字右边的数量统计出来，就是右子树的数量，然后在后序数组中从刚才的根节点往前右子树个数量范围中在递归找右子树根节点。左子树同理可得，那么完整的树就在递归中可以构建好了。
3. 那么我们可以先预处理中序数组数字对应的下标值，用哈希表映射。
4. 递归结束条件：那么中序区间左值大于区间右值返回$null$，其他返回根节点$root$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为节点数
- 空间复杂度:
  > $O(n)$，哈希表存储的节点数，其中O(h)为递归所需空间，即树的高度，这里h<n

### Code
```C++ []
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int, int> um;
        int n = inorder.size();
        for (int i = 0;i < n;++i) um[inorder[i]] = i;

        auto dfs = [&](int postorder_left, int postorder_right, int inorder_left, int inorder_right, auto&& dfs) -> TreeNode* {
            if (inorder_left > inorder_right) return nullptr;

            int postorder_root_index = postorder_right;
            int inorder_root_index = um[postorder[postorder_root_index]];
            int right_cnt = inorder_right - inorder_root_index;
            TreeNode* root = new TreeNode(postorder[postorder_root_index]);
            root->left = dfs(postorder_left, postorder_right - right_cnt - 1, inorder_left, inorder_root_index - 1, dfs);
            root->right = dfs(postorder_right - right_cnt + 1, postorder_right - 1, inorder_root_index + 1, inorder_right, dfs);
            return root;
        };
        return dfs(0, n - 1, 0, n - 1, dfs);
    }
};
```