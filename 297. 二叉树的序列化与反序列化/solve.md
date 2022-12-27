# 深度优先搜索
## 方法一（深度优先搜索）：
## 思路
1. 序列化：可以深搜先序遍历，把每个节点的值转化为字符串$"val,"$，以$","$区分，空节点可用$"null"$表示
2. 反序列化：可以先将字符串以$","$分割出来，可存入数据结构中，因为考虑到$O(1)$时间存入弹出数据，可用队列来作为存储的数据结构。然后再用深搜先序遍历做二叉树，没做一个节点弹出首位，并继续递归左子树与右子树。注意当遇到$"null"$我们用空节点返回。
## 复杂度
- 时间复杂度:
  > $O(n)$，n节点数
- 空间复杂度:
  > $O(n)$，栈空间

## Code
```C++ []
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (!root) return "null,";
        string ans;
        auto dfs = [&](TreeNode* node, auto&& dfs)->void {
            if (!node) {
                ans += "null,";
                return;
            }
            ans += to_string(node->val) + ",";
            dfs(node->left, dfs);
            dfs(node->right, dfs);
        };
        dfs(root, dfs);
        return ans;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> nodestr;
        string str;
        for (auto&& c : data) {
            if (c == ',') {
                nodestr.emplace(str);
                str.clear();
            }
            else str += c;
        }
        if (!str.empty()) nodestr.emplace(str);

        auto dfs = [&](auto&& dfs)->TreeNode* {
            if (nodestr.front() == "null") {
                nodestr.pop();
                return nullptr;
            }

            TreeNode* node = new TreeNode(stoi(nodestr.front()));
            nodestr.pop();
            node->left = dfs(dfs);
            node->right = dfs(dfs);
            return node;
        };
        return dfs(dfs);
    }
};
```
