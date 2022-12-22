# 二分查找/Z字形查找
## 方法一(二分查找)：
### 思路
1. 因为每行从左到右都是升序，我们可以对每行进行二分查找

### 复杂度
- 时间复杂度:
  > $O(mlogn)$，m，n分别为matrix的行跟列
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        for (int i = 0;i < m;++i) {
            if (binary_search(matrix[i].begin(), matrix[i].end(), target)) return true;
        }
        return false;
    }
};
```
## 方法二(Z字形查找):
### 思路
1. 因为从左到右升序，从上到下升序，我们可以从右上开始遍历，当前假如当前点为$[x,y]$有：
   - $matrix[x][y]=target$，表示找到了
   - $matrix[x][y]>target$，表示这一列所有元素都$>target$，$--y$
   - $matrix[x][y]<target$，表示这一行所有元素都$<target$，$++x$
   - 当$x>=m || y<0$退出循环
### 复杂度
- 时间复杂度:
  > $O(m+n)$，m，n分别为matrix的行跟列
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size(), x = 0, y = n - 1;
        while (x < m && y >= 0) {
            if (matrix[x][y] == target) return true;
            if (matrix[x][y] > target) --y;
            else ++x;
        }
        return false;
    }
};
```