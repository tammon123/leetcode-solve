# 哈希表/位运算/快慢指针
## 方法一(哈希表)：
### 思路
1. 哈希集合统计数据，出现重复的返回值

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(n)$，哈希表需要的空间

### Code
```C++ []
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        unordered_set<int> us;
        for (auto&& x : nums) {
            if (us.count(x)) return x;
            us.emplace(x);
        }
        return -1;
    }
};
```

## 方法二(位运算):
### 思路
1. 因为数字范围为$[1,n-1]$，而$n$最大为$10^5$，我们可以每个数的二进制。数组有$n$位，假设二进制某位数组$1$的总和为$x$，$[1,n-1]$的$1$的总和为$y$。
   - 假如只重复了一次，而重复的那个数二进制为$1$的位的$x>y$，例如：
```
nums:   1,2,3,4,2   数组总和  [1,4]总和   重复元素2二进制
第0位:  1,0,1,0,0       2         2             0
第1位:  0,1,1,0,1       3         2             1
第2位： 0,0,0,1,0       1         1             0
```
   - 假如重复超过一次，那么有数字就会被重复的数替换掉，不会出现在$[1,n-1]$中，但最终还是满足上面的条件，重复数的二进制为$1$位的$x>y$，例如：
```
nums:   1,2,2,4,2   数组总和  [1,4]总和   重复元素2二进制
第0位:  1,0,0,0,0       1         2             0
第1位:  0,1,1,0,1       3         2             1
第2位： 0,0,0,1,0       1         1             0
```

2. 那么我们可以先求出$n-1$的最高位，然后按照从低到高位进行比较，如果$x>y$，我们就让它与上$1<<bit$，最终得到重复的值
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums$的大小，$logn$为二进制位数，每次枚举位数要遍历一遍数组$O(n)$，最终需要$O(nlogn)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size();
        int max_bit = 31;
        while (!((n - 1) >> max_bit)) {
            --max_bit;
        }

        int ans = 0;
        for (int bit = 0;bit <= max_bit;++bit) {
            int x = 0, y = 0, mask = 1 << bit;
            for (int i = 0;i < n;++i) {
                if (nums[i] & mask) ++x;
                if (i >= 1 && (i & mask)) ++y;
            }
            if (x > y) ans |= mask;
        }
        return ans;
    }
};
```

## 方法二(快慢指针):
### 思路
1. 我们由$i\longrightarrow nums[i]$建图，由于有重复元素，重复元素都会指向同一个下标值，最终会构成环。

2. 成环可由快慢指针去找到环入口。假设在环内相遇位置为$b$，从开头到入环节点距离为$a$，相遇节点到入环节点为$c$，那么有两指针相遇，快指针走了n环有：$a+n(b+c)+b$，又因为快指针始终是慢指针的两倍，那么有

    $a+n(b+c)+b=2(a+b) \Longrightarrow a=c+(n-1)(b+c)$

3. 说明在两指针相遇后，我们让慢指针从头节点出发，让快指针跟慢指针同频率移动，最终会在入环点相遇
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$nums$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = 0, fast = 0;
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);
        slow = 0;
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};
```