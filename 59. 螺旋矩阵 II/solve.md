# 模拟
## 思路
1. 矩阵为数据$k∈[1,n^2]$的$n*n$螺旋矩阵，我们只需要模拟构造过程就行
2. 标记四个边界$left=0，right=n-1,top=0,bottom=n-1$，顺序遍历(左→右→下→左→上)，然后一层一层遍历，由外向内，有：
   - 先左向右，遍历赋值结束$k=n，++top$
   - 右向下，遍历赋值结束$k=2n-1，--right$
   - 下向左，遍历赋值结束$k=3n-2，--bottom$
   - 左向上，遍历赋值结束$k=4n-3, ++left$
   - 以上四次循环结束一层遍历，如果$k\le n^2$，继续往内层循环
## 复杂度
- 时间复杂度:
  > $O(n^2)$
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> ans(n, vector<int>(n));
        int left = 0, right = n - 1, top = 0, bottom = n - 1, k = 1;
        while (k <= n * n) {
            for (int i = left;i <= right;++i, ++k) ans[top][i] = k;
            ++top;
            for (int i = top;i <= bottom;++i, ++k) ans[i][right] = k;
            --right;
            for (int i = right;i >= left;--i, ++k) ans[bottom][i] = k;
            --bottom;
            for (int i = bottom;i >= top;--i, ++k) ans[i][left] = k;
            ++left;
        }
        return ans;
    }
};
```