# 深度优先搜索+广度优先搜索/KMP
## 方法一(深度优先搜索+广度优先搜索)：
### 思路
1. 通过广搜遍历$root$节点，然后深搜暴力匹配，匹配规则为，匹配当前节点，匹配当前节点左子树，匹配当前节点右子树

### 复杂度
- 时间复杂度:
  > $O(mn)$，$m$为$root$节点数，$n$为$subRoot$的节点数，暴力匹配复杂度为$O(mn)$
- 空间复杂度:
  > $O(max(m,n))$，递归栈深度不会超过$m$，广搜队列存储节点数为$n$，所以空间复杂度为这两者最大，即$O(max(m,n))$

### Code
```C++ []
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        auto dfs = [&](TreeNode* node, TreeNode* sub, auto&& dfs)->bool {
            if (!node && !sub) return true;
            else if (!node || !sub) return false;

            return node->val == sub->val && dfs(node->left, sub->left, dfs) && dfs(node->right, sub->right, dfs);
        };

        queue<TreeNode*> q;
        q.emplace(root);
        while (!q.empty()) {
            TreeNode* root = q.front(); q.pop();
            if (root->val == subRoot->val)
                if (dfs(root, subRoot, dfs)) return true;

            if (root->left) q.emplace(root->left);
            if (root->right) q.emplace(root->right);
        }
        return false;
    }
};
```
## 方法二(KMP)：
### 思路
1. 我们假如$root$与$subRoot$都通过中序遍历后，就可以用$KMP$匹配数组了，但是会忽略掉，例如:
```
  2         2
 /            \
1               1
```
这两种情况中序遍历都后都是一样的，但其实右边不是左边的子树，所以我们可以让为空节点的情况加上$lnull，rnull$，这样就能完全匹配了

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m$为$root$节点数，$n$为$subRoot$的节点数，$KMP$复杂度为$O(m+n)$
- 空间复杂度:
  > $O(m+n)$，用了双深搜搜索两棵树，栈空间最大为$O(m+n)$，存储了两棵树的节点数$O(m+n)$，$KMP$的$pi$数组$O(n)$，最终为$O(m+n)$

### Code
```C++ []
class Solution {
public:
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        int maxElement = INT_MIN;
        auto getmaxElement = [&](TreeNode* node, auto&& getmaxElement)->void {
            if (!node) return;
            maxElement = max(maxElement, node->val);
            getmaxElement(node->left, getmaxElement);
            getmaxElement(node->right, getmaxElement);
        };
        getmaxElement(root, getmaxElement);
        getmaxElement(subRoot, getmaxElement);
        int lnull = maxElement + 1, rnull = maxElement + 2;

        vector<int> vec_root, vec_subroot;
        auto dfs = [&](TreeNode* node, vector<int>& vec, auto&& dfs)->void {
            vec.emplace_back(node->val);
            if (node->left) dfs(node->left, vec, dfs);
            else vec.emplace_back(lnull);
            if (node->right) dfs(node->right, vec, dfs);
            else vec.emplace_back(rnull);
        };
        dfs(root, vec_root, dfs);
        dfs(subRoot, vec_subroot, dfs);

        int m = vec_root.size(), n = vec_subroot.size();
        vector<int> pi(n);
        for (int i = 1, j = 0;i < n;++i) {
            while (j > 0 && vec_subroot[i] != vec_subroot[j]) j = pi[j - 1];
            if (vec_subroot[i] == vec_subroot[j]) ++j;
            pi[i] = j;
        }
        for (int i = 0, j = 0;i < m;++i) {
            while (j > 0 && vec_root[i] != vec_subroot[j]) j = pi[j - 1];
            if (vec_root[i] == vec_subroot[j]) ++j;
            if (j == n) return true;
        }
        return false;
    }
};
```