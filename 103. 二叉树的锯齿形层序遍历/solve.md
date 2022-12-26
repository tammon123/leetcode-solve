# 广度优先搜索
## 思路
1. 树层次遍历可以用广搜遍历。题目要求锯齿形层遍历，也就是说，从根节点开始是左到右遍历，下一层是右到左，依次往后。我们不妨正常的遍历次序，然后在读入的时候看上一层是什么状态，下一层读取反向就行。
2. 用一个状态$state$用来记录下一次所需读取的操作，$1$表示从左到右读取，$0$表示从右到左读取
3. 我们可以把广搜用的队列换成双端队列($deque$)，那么从左到右读取就可以在$front$操作，从右到左读取可以在$back$操作。
## 复杂度
- 时间复杂度:
  > $O(n)$，n为节点数
- 空间复杂度:
  > $O(n)$，双端队列所需的存储空间

## Code
```C++ []
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root) return vector<vector<int>>();

        vector<vector<int>> ans;
        deque<TreeNode*> dq;
        dq.emplace_back(root);
        vector<int> t;
        int state = 1;
        while (!dq.empty()) {
            int size = dq.size();
            t.clear();
            auto temp = dq;
            for (int i = 0;i < size;++i) {
                TreeNode* node = state ? temp.front() : temp.back();
                state ? temp.pop_front() : temp.pop_back();
                t.emplace_back(node->val);
            }
            for (int i = 0;i < size;++i) {
                TreeNode* node = dq.front(); dq.pop_front();
                if (node->left) dq.emplace_back(node->left);
                if (node->right) dq.emplace_back(node->right);
            }
            state = state ? 0 : 1;
            ans.emplace_back(t);
        }
        return ans;
    }
};
```