# 广度优先搜索
## 方法一(广度优先搜索)：
### 思路
1. 层次遍历可以用广搜，先用队列存入根节点，然后再统计一层有多少个节点，直接遍历这么多节点数，再把每个节点对应的子节点存入队列中，如此迭代
### 复杂度
- 时间复杂度:
  > $O(n)$，n为节点数
- 空间复杂度:
  > $O(n)$，队列所存的节点数

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        if (!root) return {};
        vector<vector<int>> ans;
        queue<Node*> q;
        q.emplace(root);
        while (!q.empty()) {
            int size = q.size();
            vector<int> t;
            for (int i = 0;i < size;++i) {
                Node* node = q.front(); q.pop();
                t.emplace_back(node->val);
                for (auto&& child : node->children) {
                    q.emplace(child);
                }
            }
            if (!t.empty()) ans.emplace_back(t);
        }
        return ans;
    }
};
```