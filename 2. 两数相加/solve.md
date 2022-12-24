# 模拟
## 思路
1. 输出链表也是逆序输出，那么我们可以按位相加，满$10$进位，用个数字表示要进位的数有$add=sum/10$，这里的$sum$表示两个链表对应的位与上一进位的和
## 复杂度
- 时间复杂度:
  > $O(max(m,n))$，m位l1长度，n位l2长度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int add = 0;
        ListNode* node = new ListNode;
        ListNode* r = node;
        while (l1 || l2) {
            int x = l1 ? l1->val : 0;
            int y = l2 ? l2->val : 0;
            int sum = x + y + add;
            ListNode* t = new ListNode(sum % 10);
            r->next = t;
            r = r->next;
            add = sum / 10;
            l1 = l1 ? l1->next : l1; l2 = l2 ? l2->next : l2;
        }
        if (add != 0) {
            ListNode* t = new ListNode(add % 10);
            r->next = t;
        }
        ListNode* ans = node->next;
        delete node;
        return ans;
    }
};
```