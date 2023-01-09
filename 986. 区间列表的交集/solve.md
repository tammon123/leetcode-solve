# 归并区间
## 方法一(归并区间)：
### 思路
1. 同时取出两个区间，比较最大左端跟最小右端，如果最大左端小于等于最小右端，说明这两个区间相交，存入答案中，然后移动最小右端的下一个区间
### 复杂度
- 时间复杂度:
  > $O(m+n)$，m为数组firstList的大小，n为数组secondList的大小
- 空间复杂度:
  > $O(1)$，答案不计入复杂度

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        int m = firstList.size(), n = secondList.size(), i = 0, j = 0;
        vector<vector<int>> ans;
        while (i < m && j < n) {
            int left = max(firstList[i][0], secondList[j][0]);
            int right = min(firstList[i][1], secondList[j][1]);
            if (left <= right) {
                ans.push_back({ left, right });
            }

            if (firstList[i][1] < secondList[j][1]) ++i;
            else ++j;
        }
        return ans;
    }
};
```