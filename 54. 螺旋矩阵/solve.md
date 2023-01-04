# 模拟
## 方法一(模拟)：
### 思路
1. 标记四个边界$left=0，right=n-1,top=0,bottom=n-1$，计数$k$，顺序遍历(左→右→下→左→上)，然后一层一层遍历，由外向内，有：
   - 先左向右，遍历赋值结束$k=n，++top$
   - 右向下，遍历赋值结束$k=n+m-1，--right$
   - 下向左，遍历赋值结束$k=2n+m-2，--bottom$
   - 左向上，遍历赋值结束$k=2n+2m-3, ++left$
   - 以上四次循环结束一层遍历，如果$k<mn$，继续往内层循环
### 复杂度
- 时间复杂度:
  > $O(mn)$，m为矩阵的行，n为矩阵的列
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size(), left = 0, right = n - 1, top = 0, bottom = m - 1, k = 0,total=m*n;
        vector<int> ans;
        while (k < total) {
            for (int j = left;j <= right;++j, ++k) ans.emplace_back(matrix[top][j]);
            ++top;
            for (int i = top;i <= bottom;++i, ++k) ans.emplace_back(matrix[i][right]);
            --right;
            if(k == total) break;
            for (int j = right;j >= left;--j, ++k) ans.emplace_back(matrix[bottom][j]);
            --bottom;
            for (int i = bottom;i >= top;--i, ++k) ans.emplace_back(matrix[i][left]);
            ++left;
        }
        return ans;
    }
};
```