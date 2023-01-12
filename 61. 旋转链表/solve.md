# 闭合为环
## 方法一(闭合为环)：
### 思路
1. 旋转链表其实是闭环位移过程，只需要链表最后一个元素连接第一个元素，然后从链表最后一个元素移动$n-k\mod n$位，那么新的头节点就是移动后的下一位
2. 如果原链表为空，或者只有$1$个节点，或者$k=0$，或者$k$为$n$的倍数，原链表都不需要改变

### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表节点数
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head || k == 0 || !head->next) return head;
        int n = 1;
        ListNode* cur = head;
        while (cur->next) {
            ++n;
            cur = cur->next;
        }

        int cnt = n - k % n;
        if (cnt == n) return head;

        cur->next = head;
        while (cnt--) {
            cur = cur->next;
        }
        ListNode* ans = cur->next;
        cur->next = nullptr;
        return ans;
    }
};
```