# 最大堆+贪心/数学+贪心
## 方法一（最大堆+贪心）：
### 思路
1. 我们只需要每次从中取出最大值，第二大值减一，最终出现两个为$0$的数停止，这个过程可用最大堆模拟
### 复杂度
- 时间复杂度:
  > $O(k)$，取决于最小值与第二小值的和与最大值之间的大小对比，引申方法二
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        priority_queue<int> pq;
        pq.emplace(a); pq.emplace(b); pq.emplace(c);
        int ans = 0;
        while (!pq.empty()) {
            int p = pq.top(); pq.pop();
            int q = pq.top(); pq.pop();
            int r = pq.top();
            if (q != 0 || r != 0) {
                pq.emplace(--p); pq.emplace(--q); ++ans;
            }
            else break;
        }
        return ans;
    }
};
```

## 方法二（数学+贪心）：
### 思路
1. 假设$a<=b<=c$，那么有：
   - 如果$a+b<=c$，说明$c$能完全与$a、b$匹配，结果为$a+b$
   - 如果$a+b>c$，假设$a$与$c$匹配$k_0$，$b$与$c$匹配$k_1$，那么剩下$a-k_0$与$b-k_1$匹配最小的，即$\lfloor \frac{a-k_0+b-k_1}{2} \rfloor$，最终匹配$k_0+k_1+\lfloor \frac{a-k_0+b-k_1}{2} \rfloor$为$\lfloor \frac{a+b+c}{2} \rfloor$
### 复杂度
- 时间复杂度:
  > $O(1)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        int maxVal = max({ a,b,c }), sum = a + b + c;
        if (sum - maxVal <= maxVal) return sum - maxVal;
        else return sum / 2;
    }
};
```