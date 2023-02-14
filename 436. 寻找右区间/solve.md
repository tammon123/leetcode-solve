# 排序+二分查找/排序+双指针
## 方法一(排序+二分查找)：
### 思路
1. 题目要求每个区间找到$start_j>=end_i$最小的$start_j$，并且$start$都不一样，那么我们可以按照$start$从小到大的排序区间。这样就可以通过二分找到最小的$start_j$满足$start_j>=end_i$。

### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$intervals$的大小，排序需要$O(nlogn)$，枚举区间需要$O(n)$，每次枚举需要二分查找$O(logn)$，最终为$O(nlogn)$
- 空间复杂度:
  > $O(n)$， 数组$ids$需要的大小，排序需要空间$O(logn)$

### Code
```C++ []
class Solution {
public:
    vector<int> findRightInterval(vector<vector<int>>& intervals) {
        int n = intervals.size();
        vector<int> ids(n);
        iota(ids.begin(), ids.end(), 0);
        sort(ids.begin(), ids.end(), [&](int a, int b) {return intervals[a][0] < intervals[b][0];});
        vector<int> ans(n);
        for (int i = 0;i < n;++i) {
            int left = 0, right = n - 1, ret = -1;
            while (left <= right) {
                int mid = left + ((right - left) >> 1);
                if (intervals[ids[mid]][0] >= intervals[i][1]) {
                    ret = ids[mid];
                    right = mid - 1;
                }
                else left = mid + 1;
            }
            ans[i] = ret;
        }

        return ans;
    }
};
```
## 方法二(排序+双指针):
### 思路
1. 我们可以用两个数组分别存储开始节点与结束节点，然后对它们进行从小到大的排序。

2. 这样我们遍历结束点$end$的时候，只需要迭代找到最小的$start$节点，使得$start_j>=end_i$。继续下一个$end$，因为$end$大于等于上一个$end$，所以可以从上一次找到的$start$点继续。
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，$n$为数组$intervals$的大小，遍历节点需要$O(n)$，双排序需要$O(nlogn)$，最终趋向于$O(nlogn)$
- 空间复杂度:
  > $O(n)$，数组$startIndexs,endIndexs$需要的大小，排序需要空间$O(logn)$

### Code
```C++ []
class Solution {
public:
    using PII = pair<int, int>;
    vector<int> findRightInterval(vector<vector<int>>& intervals) {
        int n = intervals.size();
        vector<PII> startIndexs, endIndexs;
        for (int i = 0;i < n;++i) {
            startIndexs.emplace_back(intervals[i][0], i);
            endIndexs.emplace_back(intervals[i][1], i);
        }
        sort(startIndexs.begin(), startIndexs.end());
        sort(endIndexs.begin(), endIndexs.end());
        int j = 0;
        vector<int> ans(n, -1);
        for (int i = 0;i < n;++i) {
            while (j < n && startIndexs[j].first < endIndexs[i].first) {
                ++j;
            }
            if (j < n) {
                ans[endIndexs[i].second] = startIndexs[j].second;
            }
        }
        return ans;
    }
};
```