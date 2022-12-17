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
        m_m = matrix.size(), m_n = matrix[0].size();

        m_prefix_sum.resize(m_m + 1, vector<int>(m_n + 1));
        for (int i = 1;i <= m_m;++i) {
            for (int j = 1;j <= m_n;++j) {
                m_prefix_sum[i][j] = m_prefix_sum[i - 1][j] + m_prefix_sum[i][j - 1]
                    - m_prefix_sum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }

    int get(int x, int y) {
        int cx = max(min(x, m_m), 0);
        int cy = max(min(y, m_n), 0);
        return m_prefix_sum[cx][cy];
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return get(row2 + 1, col2 + 1) + get(row1, col1) - get(row1, col2 + 1) - get(row2 + 1, col1);
    }
private:
    vector<vector<int>> m_prefix_sum;
    int m_m;
    int m_n;
};
```