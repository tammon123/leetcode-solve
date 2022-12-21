# 排序
## 思路
1. 排序的思想跟题目所要求的合并思想相似。
   - $start_i<start_{i+1}$，数组从小到大排序
   - $start_i=start_{i+1}$时，按照$end_i<end_{i+1}$排序
2. 例如：
```
[1,5],[2,6],[8,10],[15,18]
```
- 第一个数组存入数据中，然后遍历第二个数组时候，用数据最后一个数组的$end_{back}$与当前遍历的$start_{new}$比较，依次类推。
  - 如果$end_{back}<start_{new}$，说明不存在合并情况，存入数据
  - 如果$end_{back}>=start_{new}$，说明存在合并情况，$end_{back}=max(end_{back},end_{new})$
## 复杂度
- 时间复杂度:
  > $O(n+nlogn)$，n为intervals的大小，nlogn为排序时间
- 空间复杂度:
  > $O(logn)$，快排所需要的空间复杂度

## Code
```C++ []
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> ans;
        for (auto&& x : intervals) {
            if (ans.empty() || ans.back()[1] < x[0]) {
                ans.push_back({ x[0], x[1] }); continue;
            }
            ans.back()[1] = max(ans.back()[1], x[1]);
        }
        return ans;
    }
};
```