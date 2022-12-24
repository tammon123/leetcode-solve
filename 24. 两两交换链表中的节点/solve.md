# 递归/迭代
## 方法一（递归）：
### 思路
1. 链表交换的截至条件为空或者只有一个节点，不能进行交换
2. 只要有两个以上节点，进行交换后，链表头节点变为第二个节点，第二个节点变为新的头节点，后面的节点可以用递归的方式交换。
3. 可令当前节点为$node$，下一个要交换的节点为$t=node.next$。那么只需要迭代，让当前节点指向后面迭代来的头节点$node.next=nextswap(t.next)$，而$t$这次交换的头结点，指向当前节点$t.next=node$，最终返回$t$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表节点数量
- 空间复杂度:
  > $O(n)$，n为递归调用空间复杂度

## Code
```C++ []
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        auto dfs = [&](ListNode* node, auto&& dfs) ->ListNode* {
            if (!node) return nullptr;
            if (node && node->next) {
                ListNode* t = node->next;
                node->next = dfs(node->next->next, dfs);
                t->next = node;
                return t;
            }
            return node;
        };
        return dfs(head, dfs);
    }
};
```
## 方法二（迭代）：
### 思路
1. 创建一个哑点$node$指向$head$，当前进行迭代节点$cur=node$
2. 下面要交换的两个节点分别为$node1=cur.next、node2=cur.next.next$，迭代条件是$cur.next$与$cur.next.next$不为空。迭代过程为：
   - 首先我们对$node1、node2$进行交换。那么我们就让$node1$指向$node2$的下一个节点$node1.next=node2.next$；$node2$指向$node1$，有$node2.next=node1$，
   - 然后让$node2$作为新的头节点$cur.next=node2$
   - 最后跳转到$node1$的位置继续迭代$cur=node1$

### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* node = new ListNode(-1, head);
        ListNode* cur = node;
        while (cur->next && cur->next->next) {
            ListNode* node1 = cur->next;
            ListNode* node2 = cur->next->next;
            node1->next = node2->next;
            node2->next = node1;
            cur->next = node2;
            cur = node1;
        }
        ListNode* ans = node->next;
        delete node;
        return ans;
    }
};
```