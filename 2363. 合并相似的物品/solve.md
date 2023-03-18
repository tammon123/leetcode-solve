# 有序映射
## 方法一(有序映射)：
### 思路
1. 用有序映射存储$value$的$weight$和，然后遍历输出数组
### 复杂度
- 时间复杂度:
  > $O((m+n)log(m+n))$，$m,n$分别为数组$items1,items2$的大小，因为有序映射内部操作需要$O(log(m+n))$，所以最终需要$O((m+n)log(m+n))$
- 空间复杂度:
  > $O(m+n)$，有序映射需要的存储大小

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) {
        map<int, int> um;
        for (auto&& x : items1) um[x[0]] += x[1];
        for (auto&& x : items2) um[x[0]] += x[1];

        vector<vector<int>> ans;
        for (auto&& [k, v] : um) ans.push_back({ k,v });
        return ans;
    }
};
```