# 哈希表/双指针
## 方法一（哈希表）：
### 思路
1. 用哈希表存储每个节点，有再次访问到，说明有环，返回节点。
### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head) return nullptr;
        unordered_set<ListNode*> us;
        us.emplace(head);
        while (head) {
            head = head->next;
            if (us.count(head)) return head;
            us.emplace(head);
        }
        return nullptr;
    }
};
```
## 方法一（双指针）：
### 思路
1. 用快慢指针来判断是否有环，有环必定会相遇。
2. 假设再环内相遇位置为$b$，从开头到入环节点距离为$a$，相遇节点到入环节点为$c$，那么有两指针相遇，快指针走了n环有：$a+n(b+c)+b$，又因为快指针始终是慢指针的两倍，那么有

    $a+n(b+c)+b=2(a+b) \Longrightarrow a=c+(n-1)(b+c)$

3. 说明在两指针相遇后，我们让一指针从头节点出发，跟慢指针一起移动，最终会在入环点相遇
### 复杂度
- 时间复杂度:
  > $O(n)$，n为链表长度
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (!head) return nullptr;
        ListNode* slow = head, * fast = head;
        while (fast) {
            slow = slow->next;
            if (fast->next == nullptr) return nullptr;
            fast = fast->next->next;
            if (slow == fast) {
                ListNode* ans = head;
                while (ans != slow) {
                    slow = slow->next;
                    ans = ans->next;
                }
                return ans;
            }
        }
        return nullptr;
    }
};
```