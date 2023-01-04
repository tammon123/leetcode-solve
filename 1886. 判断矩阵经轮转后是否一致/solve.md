# 模拟
## 方法一(模拟)：
### 思路
1. 直接采用[48. 旋转图像](https://leetcode.cn/problems/rotate-image/description/)思想转4次，每转一次判断两矩阵是否相等就行
### 复杂度
- 时间复杂度:
  > $O(n^2)$，n为矩阵大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool findRotation(vector<vector<int>>& mat, vector<vector<int>>& target) {
        int n = mat.size();
        for (int k = 0;k < 4;++k) {
            for (int i = 0;i < (n >> 1);++i) {
                for (int j = 0;j < n;++j) {
                    swap(mat[i][j], mat[n - i - 1][j]);
                }
            }

            for (int i = 0;i < n;++i) {
                for (int j = 0;j < i;++j) {
                    swap(mat[i][j], mat[j][i]);
                }
            }

            if (mat == target) return true;
        }
        return false;
    }
};
```