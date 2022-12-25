# 中序遍历
## 思路
1. 因为数组是有序的，我们可以让中间的点作为根节点，然后这样递归
## 复杂度
- 时间复杂度:
  > $O(n)$，n为数组的大小
- 空间复杂度:
  > $O(logn)$，递归所需的空间复杂度

## Code
```C++ []
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        auto dfs = [&](int begin, int end, auto&& dfs) -> TreeNode* {
            if (begin > end) return nullptr;
            int mid = begin + ((end - begin) >> 1);
            TreeNode* node = new TreeNode(nums[mid]);
            node->left = dfs(begin, mid - 1, dfs);
            node->right = dfs(mid + 1, end, dfs);
            return node;
        };
        return dfs(0, nums.size() - 1, dfs);
    }
};
```