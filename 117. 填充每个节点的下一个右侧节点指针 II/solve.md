# 迭代
## 方法一(迭代)：
### 思路
1. 我们可以采用层次遍历思想，把下一层的节点连接成一个链表。
2. 可先用哑点$dummy$开始，遍历当前层链表，从左到右，遍历它的左右子树，遍历结束，$dummy.next$就是下一层节点的开始，迭代这个过程，最终返回$root$节点

### 复杂度
- 时间复杂度:
  > $O(n)$，n为树的节点数
- 空间复杂度:
  > $O(1)$，创建了树深度个哑点

### Code
```C++ []
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return nullptr;
        Node* cur = root;
        while (cur) {
            Node* dummy = new Node(-1);
            Node* pre = dummy;
            while (cur) {
                if (cur->left) {
                    pre->next = cur->left;
                    pre = pre->next;
                }
                if (cur->right) {
                    pre->next = cur->right;
                    pre = pre->next;
                }
                cur = cur->next;
            }
            cur = dummy->next;
            delete dummy;
        }
        return root;
    }
};
```