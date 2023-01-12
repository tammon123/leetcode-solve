# 反转链表/栈
## 方法一(反转链表)：
### 思路
1. 先把两链表反转，然后再用竖式相加法，因为竖式相加后还要反转，我们可以直接采用反转链表方法把链表创建起来

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m$为链表$l1$的长度，$n$为链表$l2$的长度
- 空间复杂度:
  > $O(1)$，没有额外使用空间

### Code
```C++ []
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* cur = l1;
        ListNode* pre = nullptr;
        while (cur) {
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        cur = l2;
        ListNode* next = nullptr;
        while (cur) {
            ListNode* temp = cur->next;
            cur->next = next;
            next = cur;
            cur = temp;
        }

        int add = 0;
        ListNode* ans = nullptr;
        while (pre || next || add > 0) {
            int x = pre ? pre->val : 0;
            int y = next ? next->val : 0;
            int r = x + y + add;
            ListNode* node = new ListNode(r % 10);
            node->next = ans;
            ans = node;
            add = r / 10;
            pre ? pre = pre->next : pre;
            next ? next = next->next : next;
        }

        return ans;
    }
};
```
## 方法二(栈)：
### 思路
1. 如果不改变原链表数据，那么我们可以用栈，把链表数据插入，那么运算就是从后往前，起到反转作用

### 复杂度
- 时间复杂度:
  > $O(m+n)$，$m$为链表$l1$的长度，$n$为链表$l2$的长度
- 空间复杂度:
  > $O(m+n)$，栈要存储两个链表数据

### Code
```C++ []
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<ListNode*> st_l1, st_l2;
        while (l1) {
            st_l1.emplace(l1);
            l1 = l1->next;
        }
        while (l2) {
            st_l2.emplace(l2);
            l2 = l2->next;
        }

        int add = 0;
        ListNode* ans = nullptr;
        while (!st_l1.empty() || !st_l2.empty() || add > 0) {
            int x = 0, y = 0;
            if (!st_l1.empty()) x = st_l1.top()->val, st_l1.pop();
            if (!st_l2.empty()) y = st_l2.top()->val, st_l2.pop();
            int r = x + y + add;
            ListNode* node = new ListNode(r % 10);
            node->next = ans;
            ans = node;
            add = r / 10;
        }
        return ans;
    }
};
```