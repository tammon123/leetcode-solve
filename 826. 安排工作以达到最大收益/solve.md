# 排序
## 方法一(排序)：
### 思路
1. 工人只能完成难度小于或等于$worker[i]$的工作，那么把难度与收益结合按照从小到大的顺序排序，然后工人也从小到达排序。

2. 那么我们遍历工人时候找到难度匹配，下一个工人可以再上一个查询后接着往下查询。查询难度时候，找出难度下的最大收益。

### 复杂度
- 时间复杂度:
  > $O(nlogn+mlogm)$，$n$为数组$difficulty$的大小，$m$为数组$worker$的大小，遍历$difficulty$数组需要$O(n)$，排序需要$O(nlogn+mlogm)$，统计结果需要$O(m+n)$
- 空间复杂度:
  > $O(n)$，数组$jobs$需要大小，排序需要的空间为$O(logn+logm)$

### Code
```C++ []
class Solution {
public:
    using PII = pair<int, int>;
    int maxProfitAssignment(vector<int>& difficulty, vector<int>& profit, vector<int>& worker) {
        int n = difficulty.size(), ans = 0;
        vector<PII> jobs(n);
        for (int i = 0;i < n;++i) {
            jobs[i] = { difficulty[i],profit[i] };
        }
        sort(jobs.begin(), jobs.end());
        sort(worker.begin(), worker.end());
        int last = 0, maxm = 0;
        for (auto&& x : worker) {
            while (last < n && x >= jobs[last].first) {
                maxm = max(maxm, jobs[last++].second);
            }
            ans += maxm;
        }
        return ans;
    }
};
```