# 栈/寻找中心点+链表逆序+合并链表
## 方法一（哈希表）：
### 思路
1. 链表从前面与后面交叉链接，那我们可以通过一次遍历，用栈存储链表节点达到逆序节点，计算节点数。然后再遍历就可以交叉链接，节点数够了停止遍历。
2. 注意遍历节点结束时候，如果不为空，让它下一个节点指向$null$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表节点数
- 空间复杂度:
  > $O(n)$， 栈存储链表节点数

## Code
```C++ []
class Solution {
public:
    void reorderList(ListNode* head) {
        int cnt = 0;
        stack<ListNode*> st;
        ListNode* cur = head;
        while (cur) {
            st.emplace(cur);
            cur = cur->next;
            ++cnt;
        }

        ListNode* node = new ListNode(-1);
        cur = node;
        while (cnt) {
            cur->next = head;
            cur = cur->next;
            head = head->next;
            --cnt;
            if (cnt == 0) break;
            cur->next = st.top();
            st.pop();
            cur = cur->next;
            --cnt;
        }
        if (cur) cur->next = nullptr;
        ListNode* ans = node->next; delete node;
        head = ans;
    }
};
```
## 方法二（寻找中心点+链表逆序+合并链表）：
### 思路
1. 由方法一可知，我们正序跟逆序链表最终会汇聚与中心点，我们可以先找到中心点，然后中心点下一个节点开始反转链表，最后两链表合并
    - 找中心点可以参照[876. 链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/)
    - 反转链表可以[206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)
    - 两链表长度不会超过$1$，因此可以直接合并

### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    void reorderList(ListNode* head) {
         //找中心点
        ListNode* slow = head, * fast = head;
        while (fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        ListNode* mid = slow;

        //反转链表
        ListNode* right = mid->next;
        mid->next = nullptr;
        ListNode* prev = nullptr;
        while (right) {
            ListNode* temp = right->next;
            right->next = prev;
            prev = right;
            right = temp;
        }
        right = prev;

        ListNode* left = head;
        //合并链表left,right
        while (left && right) {
            ListNode* node1 = left->next;
            ListNode* node2 = right->next;
            left->next = right;
            left = node1;
            right->next = left;
            right = node2;
        }
    }
};
```