# 二分查找
## 思路
1. 因为每行从左到右升序，下一行开头大于前一行结尾，整个矩阵可以看作是单调递增的，可直接二分查找
## 复杂度
- 时间复杂度:
  > $O(logmn)$，m，n分别为matrix的行跟列
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size(), l = 0, r = m * n - 1;
        while (l <= r) {
            int mid = l + ((r - l) >> 1);
            if (matrix[mid / n][mid % n] == target) return true;
            else if (matrix[mid / n][mid % n] > target) r = mid - 1;
            else l = mid + 1;
        }
        return false;
    }
};
```