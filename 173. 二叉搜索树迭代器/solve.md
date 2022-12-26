# 栈+中序遍历
## 思路
1. 可以利用栈对搜索树进行中序遍历，可以预先存根节点与左子树节点，然后调用$next()$时候判断当前节点有没有右子树，有就同样中序遍历存根节点与左子树
2. $hasNext()$通过判断栈是否为空，$O(1)$时间调用
## 复杂度
- 时间复杂度:
  > $O(n)$，hasNext为O(1), next最坏复杂度，是线性树需要O(n)时间，考虑最终会调用n次next，平均为O(1)
- 空间复杂度:
  > $O(n)$，n取决于栈深度，线性树会到n

## Code
```C++ []
class BSTIterator {
public:
    BSTIterator(TreeNode* root) {
        st.emplace(root);
        while (root->left) {
            root = root->left;
            st.emplace(root);
        }
    }

    int next() {
        TreeNode* node = st.top(); st.pop();
        int result = node->val;
        if (node->right) {
            node = node->right;
            st.emplace(node);
            while (node->left) {
                node = node->left;
                st.emplace(node);
            }
        }
        
        return result;
    }

    bool hasNext() {
        return !st.empty();
    }
private:
    stack<TreeNode*> st;
};
```