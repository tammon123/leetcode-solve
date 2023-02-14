# 二分查找+位运算
## 方法一(二分查找+位运算)：
### 思路
1. 根据定义可知，最底层最左的叶子节点一定存在，我们可以通过一直迭代左子树从而知道树的层数$level$。到达最大层后节点数一定在范围$[2^i,2^{i+1}-1]$，$i$从$0$开始。

2. 那么我们可以二分这个范围，查找最右的叶子点是否存在。判断叶子是否存在，我们可以把树看成二进制，根节点为一个数最高点$1$，从根节点位开始到低位，$1$表示右子树，$0$表示左子树。例如数字$12$在树上的位置，我们可以先将$12$二进制为```1100```，那么除了最高位$1$，那么从根节点出发，向右子树走，然后左子树走，最后左子树到达$12$所在位置。

### 复杂度
- 时间复杂度:
  > $O(log^2n)$，$n$为树$root$节点数，计算高度需要$O(logn)$，最底层需要二分$O(log2^h)=O(h)$，每次二分需要遍历从根节点到目标节点$O(h)$，所以需要$O(h^2)$，因为$2^h<=n<2^{h+1}$，所以$O(h^2)=O(log^2n)$
- 空间复杂度:
  > $O(1)$

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
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        int level = 0;
        TreeNode* cur = root;
        while (cur->left) {
            cur = cur->left;
            ++level;
        }

        auto exists = [&](TreeNode* node, int x)->bool {
            if (level == 0) return true;
            int mask = 1 << (level - 1);
            while (node && mask) {
                if (!(mask & x)) {
                    node = node->left;
                }
                else node = node->right;
                mask >>= 1;
            }
            return node != nullptr;
        };

        int left = 1 << level, right = (1 << (level + 1)) - 1, ans = 0;
        while (left <= right) {
            int mid = left + ((right - left) >> 1);
            if (exists(root, mid)) {
                ans = mid;
                left = mid + 1;
            }
            else right = mid - 1;
        }
        return ans;
    }
};
```