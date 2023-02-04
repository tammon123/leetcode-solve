# 自定义排序
## 方法一(自定义排序)：
### 思路
1. 因为矩阵最终每一行按照第$k$列排序，我们可以自定义排序，按照第$k$列排序行就行

### 复杂度
- 时间复杂度:
  > $O(mlogm)$，$m$为矩阵$score$的行大小，排序要$mlogm$时间
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> sortTheStudents(vector<vector<int>>& score, int k) {
        sort(score.begin(), score.end(), [&](const vector<int>& a, const vector<int>& b) {return *(a.begin() + k) > *(b.begin() + k);});
        return score;
    }
};
```
