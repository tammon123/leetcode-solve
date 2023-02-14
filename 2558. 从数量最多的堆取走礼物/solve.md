# 最大堆
## 方法一(最大堆)：
### 思路
1. 最大堆模拟$k$次就行，如果$k$值很大，可以判断堆顶如果为$1$可以停止模拟了

### 复杂度
- 时间复杂度:
  > $O(n+klogn)$，$n$为数组$gifts$的大小，先入堆$O(n)$，需要迭代$k$次，每次堆操作为$O(logn)$
- 空间复杂度:
  > $O(n)$，堆需要存储$n$个数据

### Code
```C++ []
class Solution {
public:
    long long pickGifts(vector<int>& gifts, int k) {
        priority_queue<int> pq;
        for (auto&& x : gifts) {
            pq.emplace(x);
        }
        while (k && pq.top() != 1) {
            int x = pq.top();
            pq.pop();
            x = sqrt(x);
            pq.emplace(x);
            --k;
        }
        long long ans = 0ll;
        while (!pq.empty()) {
            ans += pq.top();
            pq.pop();
        }
        return ans;
    }
};
```