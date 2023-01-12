# 二维前缀和+动态规划
## 思路
1. 思路参考1314题目

### 复杂度
- 时间复杂度:
  > $O(mn)$
- 空间复杂度:
  > $O(mn)$

### Code
```C++ []
class NumMatrix {
public:
    NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();

        m_prefix_sum.resize(m + 1, vector<int>(n + 1));
        for (int i = 1;i <= m;++i) {
            for (int j = 1;j <= n;++j) {
                m_prefix_sum[i][j] = m_prefix_sum[i - 1][j] + m_prefix_sum[i][j - 1]
                    - m_prefix_sum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }

    int get(int x, int y) {
        return m_prefix_sum[x][y];
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return get(row2 + 1, col2 + 1) + get(row1, col1) - get(row1, col2 + 1) - get(row2 + 1, col1);
    }
private:
    vector<vector<int>> m_prefix_sum;
};
```