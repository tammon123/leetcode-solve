# 排序+最大堆
## 方法一(排序+最大堆)：
### 思路
1. 每次取最大元素，然后进行$nums[i]=ceil(nums[i] / 3)$操作，那我们可以想到用最大堆存储元素，每次取最大元素增加值后再重新入堆
2. 我们可以优化这个过程：
   - 可以先将数组从大到小的排序，用一个指针$p$指向首位元素，加入答案后，操作入堆，将$k$减一，$++p$
   - 然后迭代$k$值，直到堆顶元素为$1$时，为啥要判断为$1$，因为最后都是重复$k$遍，我们结束循环，然后答案加上$k+1$就行
   - 迭代过程，可以对比$nums[p]$与堆顶元素大小，如果数组元素大，答案增加数组值，操作入堆，指针$p$加一。如果堆顶元素大，答案增加堆顶值，弹出堆顶元素，操作入堆

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$nums$大小，排序需要$O(nlogn)$，堆内部操作$logk$，迭代$k$所以需要$O(klogk)$，如果$k$远大于$n$，因为判断堆顶为$1$结束迭代优化过程，所以$O(klogk)$逼近为$O(nlogn)$，最终为$O(nlogn)$
- 空间复杂度:
  > $O(n)$，堆最多存储$n$个元素

### Code
```C++ []
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        priority_queue<int> pq;
        sort(nums.begin(), nums.end(), greater<int>());
        int p = 0, n = nums.size();
        long long ans = 0ll;
        --k;
        ans += nums[p];
        pq.emplace(ceil(nums[p++] * 1.0 / 3));
        while (k-- && pq.top() != 1) {
            if (p < n && nums[p] >= pq.top()) {
                ans += nums[p];
                pq.emplace(ceil(nums[p++] * 1.0 / 3));
            }
            else {
                int val = pq.top(); pq.pop();
                ans += val;
                pq.emplace(ceil(val * 1.0 / 3));
            }
        }
        if (k > 0) ans += k + 1;
        return ans;
    }
};
```