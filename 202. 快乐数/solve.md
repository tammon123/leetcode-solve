# 哈希表
## 方法一(位移)：
### 思路
1. 按照快乐数的定义，最终操作后要么变为$1$，要么跌入循环

2. 那么我们只要用哈希表记录每次操作数据，如果有重复说明最终会重复然后进入循环。反之会迭代成$1$

3. 那么循环迭代会不会无限增加呢，其实不会，因为数字超过$4$位后，会最终迭代到$3$位内进行循环迭代。

### 复杂度
- 时间复杂度:
  > $O(logn)$，每次位数操作需要$O(logn)$复杂度，下次操作取决于上次$O(logn)$复杂度后得到的值，往后迭代，那么最大趋于$O(logn)$
- 空间复杂度:
  > $O(logn)$，对于大数字$n$，最终会迭代到$3$位内，空间最终也会趋于$O(logn)$

### Code
```C++ []
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> us;
        us.emplace(n);

        auto happy = [&](int x)->int {
            int ret = 0;
            while (x) {
                int k = x % 10;
                ret += k * k;
                x /= 10;
            }
            return ret;
        };

        while (n != 1) {
            n = happy(n);
            if (us.count(n)) return false;
            us.emplace(n);
        }
        return true;
    }
};
```

## 方法二(快慢指针法)：
### 思路
1. 按照方法一思想，循环以链表的看法，最终是成环，我们判断成环条件可以用快慢指针法，如果快慢指针相等说明跌入循环了，否则会迭代为$1$

### 复杂度
- 时间复杂度:
  > $O(logn)$，快慢指针复杂度为$O(logn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool isHappy(int n) {
        auto happy = [&](int x)->int {
            int ret = 0;
            while (x) {
                int k = x % 10;
                ret += k * k;
                x /= 10;
            }
            return ret;
        };

        int slow = n, fast = happy(n);
        while (fast != 1 && slow != fast) {
            slow = happy(slow);
            fast = happy(happy(fast));
        }
        return fast == 1;
    }
};
```