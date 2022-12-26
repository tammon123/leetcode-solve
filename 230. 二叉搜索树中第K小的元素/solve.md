# 中序遍历
## 思路
1. 二叉搜索树的特性，根节点大于左子树所有点，根节点小于左子树所有点。
2. 我们可以通过中序遍历，找到第k个最小的值
## 复杂度
- 时间复杂度:
  > $O(h+k)$，h为树的高度，当树是平衡树时，时间复杂度最小为O(logn+k)，当树是线性树时，时间复杂度最大为O(n)
- 空间复杂度:
  > $O(h)$，n取决于栈空间开销

## Code
```C++ []
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*> st;
        while (root || !st.empty()) {
            while (root) {
                st.emplace(root);
                root = root->left;
            }
            root = st.top(); st.pop();
            --k;
            if (k == 0) break;
            root = root->right;
        }
        return root->val;
    }
};
```