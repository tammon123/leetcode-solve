# 哈希表/一次遍历
## 方法一（哈希表）：
### 思路
1. 遍历链表用哈希表存储节点值跟数量，再次遍历链表把值为$1$的存入新链表中
### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(n)$

## Code
```C++ []
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return nullptr;
        ListNode* t = head;
        unordered_map<int, int> um;
        while (t) {
            ++um[t->val];
            t = t->next;
        }
        t = head;
        ListNode* node = new ListNode(-1);
        ListNode* cur = node;
        while (t) {
            if (um[t->val] == 1) cur->next = t, cur = cur->next;
            t = t->next;
        }
        if (cur) cur->next = nullptr;
        ListNode* ans = node->next;
        delete node;
        return ans;
    }
};
```
## 方法二（一次遍历）：
### 思路
1. 因为链表节点值都是排序好的，我们只需要把后一个节点跟它值相同的节点删除就行,具体操作如下：
    - 先创建一个新链表头节点，值为$-1$，并让它的下一个节点指向$head$
    - 用过程$cur$节点指向这个新节点，然后判断$cur.next$与$cur.next.next$的值是否相等，如果相等，我们记$x=cur.next.val$，通过迭代不断删除$cur.next$与$cur.next$相等的后面所有节点，直到$cur.next$为空或者与$x$值不等的节点。
    - 如果$cur.next$与$cur.next.next$的值不相等，那么就让$cur$指向下一个节点。
    - 最终表头结点的下一个结点就是要求的链表

### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return nullptr;
        ListNode* node = new ListNode(-1, head);
        ListNode* cur = node;
        while (cur->next && cur->next->next) {
            if (cur->next->val == cur->next->next->val) {
                int x = cur->next->val;
                while (cur->next && cur->next->val == x) {
                    cur->next = cur->next->next;
                }
            }
            else cur = cur->next;
        }
        ListNode* ans = node->next;
        delete node;
        return ans;
    }
};
```