# 哈希表+模拟/脑经急转弯
## 方法一(哈希表+模拟)：
### 思路
1. 直接按照题意模拟，然后用哈希集合记录数字防止重复计数

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为给定数，每次都从$1$遍历到要计算的点，趋于$n^2$
- 空间复杂度:
  > $O(n)$，哈希表跟队列存储$n$个大小

### Code
```C++ []
class Solution {
public:
    int distinctIntegers(int n) {
        queue<int> q;
        q.emplace(n);
        unordered_set<int> us;
        int ans = 1;
        us.emplace(n);
        while (!q.empty()) {
            int x = q.front(); q.pop();
            for (int i = 1;i < x;++i) {
                if (x % i == 1 && !us.count(i)) {
                    q.emplace(i);
                    us.emplace(i);
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

## 方法二(脑经急转弯)：
### 思路
1. $n$开始求余为$1$的点，那么下次$n-1$必定再桌面上，最终有$n-1$个点

2. 如果$n=1$，只有$1$个点

### 复杂度
- 时间复杂度:
  > $O(1)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int distinctIntegers(int n) {
        return n == 1 ? 1 : n - 1;
    }
};
```
