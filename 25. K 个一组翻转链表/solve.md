# 递归
## 方法一（递归）：
### 思路
1. 因为是$k$倍节点进行一次反转，我们可以先找到第$k$个节点，然后反转链表，$k+1$后节点用递归实现。
2. 需要注意再找$k$个节点时候，如果不满足$k$个节点，那么就返回当前节点。
### 复杂度
- 时间复杂度:
  > $O(n)$，n节点数
- 空间复杂度:
  > $O(logn)$，递归栈空间

### Code
```C++ []
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        auto dfs = [&](ListNode* node, auto&& dfs)->ListNode* {
            if (!node) return nullptr;
            ListNode* cur = node;
            int cnt = 1;
            while (cur) {
                if (cnt == k) break;
                cur = cur->next;
                ++cnt;
            }
            if(cnt == k && !cur || cnt < k) return node;
            ListNode* tail = dfs(cur->next, dfs);

            ListNode* pre = tail;
            cur = node;
            while (cnt) {
                ListNode* temp = cur->next;
                cur->next = pre;
                pre = cur;
                cur = temp;
                --cnt;
            }
            return pre;
        };
        return dfs(head, dfs);
    }
};
```
