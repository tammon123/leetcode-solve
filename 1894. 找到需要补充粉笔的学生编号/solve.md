# 求余模拟/前缀和+二分查找
## 方法一(求余模拟)：
### 思路
1. 我们可以求出所有的和，然后对$k$求余后再遍历
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$chalk$的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int chalkReplacer(vector<int>& chalk, int k) {
        int n = chalk.size();
        long sum = reduce(chalk.begin(), chalk.end(), 0l);
        k %= sum;
        for (int i = 0;i < n;++i) {
            k -= chalk[i];
            if (k < 0) return i;
        }
        return 0;
    }
};
```

## 方法二(前缀和+二分查找)：
### 思路
1. 可以求出前缀和，求余后，二分查找和超过$k$的第一个数的下标
### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为数组$chalk$的大小，二分需要$logn$复杂度
- 空间复杂度:
  > $O(n)$，前缀和存储大小

### Code
```C++ []
class Solution {
public:
    int chalkReplacer(vector<int>& chalk, int k) {
        int n = chalk.size();
        vector<long long> presum(n + 1);
        for (int i = 0;i < n;++i) {
            presum[i + 1] = presum[i] + chalk[i];
        }
        k %= presum[n];
        return upper_bound(presum.begin(), presum.end(), k) - presum.begin() -1;
    }
};
```