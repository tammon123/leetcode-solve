# 后序遍历/哈希表+深度优先遍历
## 方法一（后序遍历）：
### 思路
1. 我们可以通过后序遍历思想找到最近公共祖先，先遍历左子树如果包含$p$，那么$q$要么在右子树，要在根节点。为啥这是正确的呢，因为是自底向上的遍历，如果满足左右子树包含$p、q$那么最近的根节点一定是最近公共祖先。
2. 需要满足的条件为：$bleft \&\& bright||((node.val=q.val||node.val=p.val)\&\&(bleft||bright))$，$bleft$表示左子树包含$p、q$，$bright$表示右子树包含$p、q$，$node$表示当前遍历的节点
### 复杂度
- 时间复杂度:
  > $O(n)$，n表示节点数，最大为遍历所有节点
- 空间复杂度:
  > $O(n)$，递归栈深度，最高变成线性表n

### Code
```C++ []
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        TreeNode* ans;
        auto dfs = [&](TreeNode* node, auto&& dfs) ->bool {
            if (!node) return false;
            bool bleft = dfs(node->left, dfs);
            bool bright = dfs(node->right, dfs);
            if ((bleft && bright) || ((node->val == p->val || node->val == q->val) && (bleft || bright))) ans = node;
            return bleft || bright || node->val == p->val || node->val == q->val;
        };
        dfs(root, dfs);
        return ans;
    }
};
```
## 方法二（哈希表+深度优先探索）：
### 思路
1. 因为每个值都不一样，可以作为哈希表的键值，然后把父节点存入当中，然后回溯$p$的父节点标记，用哈希表存储，同样回溯$q$的父节点，如果有访问，说明这个节点就是最近公共祖先
### 复杂度
- 时间复杂度:
  > $O(n)$，n表示节点数会先全部遍历一遍，因为p、q回溯父节点不会超过n
- 空间复杂度:
  > $O(n)$，递归栈深度，最高变成线性表n，哈希表存储的父节点不会超过n，标记哈希也不会超过n

### Code
```C++ []
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        unordered_map<int, TreeNode*> um;
        unordered_set<TreeNode*> us;
        auto dfs = [&](TreeNode* node, auto&& dfs)->void {
            if (!node) return;
            if (node->left) um[node->left->val] = node;
            if (node->right) um[node->right->val] = node;
            dfs(node->left, dfs);
            dfs(node->right, dfs);
        };
        dfs(root, dfs);

        while (p) {
            us.emplace(p);
            p = um[p->val];
        }
        while (q) {
            if (us.count(q)) return q;
            q = um[q->val];
        }
        return nullptr;
    }
};
```