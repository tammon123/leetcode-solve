# 迭代+哈希表
## 方法一(迭代+哈希表)：
### 思路
1. 可以先遍历一遍链表$head$，然后深拷贝链表$dummy$，并将每一个链表$head$节点映射为对应链表$dummy$节点存入哈希表中。
2. 然后再次遍历链表$head$，就能知道每个节点的随机节点是哪个节点了。注意随机节点为空节点情况

### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表$head$的节点数
- 空间复杂度:
  > $O(n)$，哈希表存储链表节点数$n$

### Code
```C++ []
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return nullptr;

        Node* dummy = new Node(-1);
        Node* cur = dummy, * temp_head = head;
        unordered_map<Node*, Node*> um;//key: head, val: dummy
        while (temp_head) {
            Node* newnode = new Node(temp_head->val);
            cur->next = newnode;
            um[temp_head] = newnode;
            temp_head = temp_head->next;
            cur = cur->next;
        }
        cur = dummy->next;
        temp_head = head;
        while (temp_head) {
            if (!temp_head->random) cur->random = nullptr;
            else cur->random = um[temp_head->random];
            temp_head = temp_head->next;
            cur = cur->next;
        }
        Node* ans = dummy->next;
        delete dummy;
        return ans;
    }
};
```