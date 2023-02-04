# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 用深搜遍历一遍树，求出总节点个数$total$与目标节点的左子树个数$target_{left}$与右子树个数$target_{right}$

2. 因为要想获得胜利，那么再目标节点后，要么再它父节点着色，要么再它左右子树数量最多的区域着色。那么只要满足**总数大于目标节点左右子树的和加目标节点个数$1$的和两倍**或者**目标节点的左右子树个数的最大值大于总目标节点个数减去这个最大值后的值**，表示蓝色着色能胜利。即胜利条件为:$total>2*(target_{left}+target_{right}+1)||2*max(target_{left},target_{right})>total$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$给定的节点数，需要遍历整棵树
- 空间复杂度:
  > $O(n)$，递归栈需要的最坏复杂度为一条直线$O(n)$

### Code
```C++ []
class Solution {
public:
    bool btreeGameWinningMove(TreeNode* root, int n, int x) {
        int total = 0, target_left = 0, target_right = 0;
        auto dfs = [&](TreeNode* node, auto&& dfs)->int {
            if (!node) return 0;
            int ret = 1;
            int left = dfs(node->left, dfs);
            int right = dfs(node->right, dfs);
            if (node->val == x) target_left = left, target_right = right;
            return ret + left + right;
        };
        total = dfs(root, dfs);
        return total > 2 * (target_left + target_right + 1) || 2 * max(target_left, target_right) > total;
    }
};
```