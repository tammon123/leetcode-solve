# 原地旋转/翻转
## 方法一（原地旋转）：
### 思路
1. 直接原地交换如下：
$
\begin{cases}
  &  matrix[i][j]&=&matrix[n - j - 1][i] \\
  &  matrix[n - j - 1][i]&=&matrix[n - i - 1][n - j - 1] \\
  &  matrix[n - i - 1][n - j - 1]&=&matrix[j][n - i - 1] \\
  &  matrix[j][n - i - 1]&=&matrix[i][j]
\end{cases}
$
2. 矩阵需要遍历多少才能全部旋转呢。
   - 假如$n$是偶数，我们需要遍历$\frac{n}{2}*\frac{n}{2}$个元素就能完成全部旋转
   - 假如$n$是奇数，我们需要遍历$\frac{n-1}{2}*\frac{n+1}{2}$个元素就能完成旋转
### 复杂度
- 时间复杂度:
  > $O(n^2)$，n为matrix边长
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0;i < (n >> 1);++i) {
            for (int j = 0;j < ((n + 1) >> 1);++j) {
                tie(matrix[i][j], matrix[j][n - i - 1], matrix[n - i - 1][n - j - 1], matrix[n - j - 1][i]) = \
                    make_tuple(matrix[n - j - 1][i], matrix[i][j], matrix[j][n - i - 1], matrix[n - i - 1][n - j - 1]);
            }
        }
    }
};
```

## 方法二（翻转）：
### 思路
1. 矩阵旋转$90°$后，元素变化有：$matrix[row][col]=>matrix[col][n-row-1]$
2. 那我们等价于
   - 可以先把矩阵水平交换：$matrix[row][col]=>matrix[n-row-1][col]$
   - 再进行对角线交换：$matrix[n-row-1][col]=>matrix[col][n-row-1]$
### 复杂度
- 时间复杂度:
  > $O(n^2)$
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        for (int i = 0;i < (n >> 1);++i) {
            for (int j = 0;j < n;++j) {
                swap(matrix[i][j], matrix[n - i - 1][j]);
            }
        }

        for (int i = 0;i < n;++i) {
            for (int j = 0;j < i;++j) {
                swap(matrix[i][j], matrix[j][i]);
            }
        }
    }
};
```