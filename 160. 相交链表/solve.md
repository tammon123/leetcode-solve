# 哈希表/双指针
## 方法一（哈希表）：
### 思路
1. 用哈希表存储节点，遍历两个链表，如果重复读取，说明有相交节点。没有返回$null$
### 复杂度
- 时间复杂度:
  > $O(m+n)$，m为链表listA的长度，n为链表listB的长度
- 空间复杂度:
  > $O(m+n)$

## Code
```C++ []
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set<ListNode*> us;
        us.emplace(headA);
        if (us.count(headB)) return headB;
        us.emplace(headB);
        while (headA) {
            headA = headA->next;
            if (us.count(headA)) return headA;
            us.emplace(headA);
        }
        while (headB) {
            headB = headB->next;
            if (us.count(headB)) return headB;
            us.emplace(headB);
        }
        return nullptr;
    }
};
```
## 方法二（双指针）：
### 思路
1. 用双指针$a、b$分别指向链表$headA、headB$的开始，同时遍历。如果指针$a$先遍历结束，让$a$指向$headB$，$b$结束，让$b$指向$headA$，如果相交，最终会有共同点，如果不相交，会共同到结尾。
2. 假设，链表$headA、headB$的长度分别为$m、n$，$headA、headB$相交有$x$个点，$headA$不相交$y$个点，$headB$不相交有$z$个点，那么有：$x+y=m，x+z=n$
- 情况一，两链表相交：
    - 如果$m=n$，那么两链表会交于一点
    - 如果$m\neq n$， 那么它们遍历结尾出，然后交叉遍历，最终$headA$会在$x+y+z$与$headB$在$x+z+y$出相遇
- 情况二，两链表不相交：
    - 如果$m=n$，会同时遍历到结尾，返回$null$
    - 如果$m\neq n$，因为没有公共节点，那么两个链表在交互遍历完到结尾，即$x+y+x+z=2(m+n)$时，返回$null$

### 复杂度
- 时间复杂度:
  > $O(m+n)$，m为链表listA的长度，n为链表listB的长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        unordered_set<ListNode*> us;
        us.emplace(headA);
        if (us.count(headB)) return headB;
        us.emplace(headB);
        while (headA) {
            headA = headA->next;
            if (us.count(headA)) return headA;
            us.emplace(headA);
        }
        while (headB) {
            headB = headB->next;
            if (us.count(headB)) return headB;
            us.emplace(headB);
        }
        return nullptr;
    }
};
```