# 深度优先搜索
## 方法一(深度优先搜索)：
### 思路
1. 先用字符串+分隔符$"，"$把链表串联起来
2. 然后深搜二叉树，同样用字符串连接各节点结果，采用回溯的方法判断链表是否在二叉树中
### 复杂度
- 时间复杂度:
  > $O(m+n*mn)$，m为链表head的长度，n为二叉树节点数，O(mn)为string.find的时间复杂度
- 空间复杂度:
  > $O(m+n)$，字符串所存储的链表跟二叉树的长度

### Code
```C++ []
class Solution {
public:
    bool isSubPath(ListNode* head, TreeNode* root) {
        string strList;
        while (head) {
            strList += to_string(head->val) + ",";
            head = head->next;
        }

        string strTree;
        auto dfs = [&](TreeNode* node, auto&& dfs)->bool {
            if (!node) {
                return strTree.find(strList) != string::npos;
            }
            string temp = to_string(node->val) + ",";
            int size = temp.size();
            strTree += temp;
            bool left = dfs(node->left, dfs);
            bool right = dfs(node->right, dfs);
            if (left || right) return true;
            for (int i = 0;i < size;++i) strTree.pop_back();
            return false;
        };
        return dfs(root, dfs);
    }
};
```