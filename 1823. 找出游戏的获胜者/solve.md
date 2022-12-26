# 队列/约瑟夫环
## 方法一（队列）：
### 思路
1. 先遍历让$[1,n]$数字入队，然后顺序出队，如果出队次数等于$k$，不再入队，反之入队，直到最后剩余一个人就是要求的。
### 复杂度
- 时间复杂度:
  > $O(nk)$，每轮出k个人，要进行n-1轮
- 空间复杂度:
  > $O(n)$，队列最多存储n个人

### Code
```C++ []
class Solution {
public:
    int findTheWinner(int n, int k) {
        if (n == 1 || k == 1) return n;
        queue<int> q;
        for (int i = 1;i <= n;++i) q.emplace(i);

        int cnt = 1;
        while (q.size() != 1) {
            int front = q.front(); q.pop();
            if (cnt != k) q.emplace(front), ++cnt;
            else cnt = 1;
        }
        return q.front();
    }
};
```

## 方法一（约瑟夫环）：
### 思路
1. 这题是约瑟夫环数应用题，公式有：$f[1]=0，f[i]=(f[i-1]+m)\mod i (i>1)$
2. 因为这题从$1$开始，所以$f[1]=1，f[n]=(f[n-1]+k-1)\%n+1$
### 复杂度
- 时间复杂度:
  > $O(n)$，每个值计算时间都是O(1)，所以是n
- 空间复杂度:
  > $O(n)$，取决于递归调用深度n层

### Code
```C++ []
class Solution {
public:
    int findTheWinner(int n, int k) {
        if (n == 1) return 1;
        return (findTheWinner(n - 1, k) + k - 1) % n + 1;
    }
};
```