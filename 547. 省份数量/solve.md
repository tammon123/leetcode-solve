# 深度优先搜索/广度优先搜索/并查集
## 方法一(深度优先搜索)：
### 思路
1. 深搜遇到$'1'$，答案加一，然后遍历关联行数，裁剪相关的$'1'$都变为$'0'$，即:$isConnected[i][j] = isConnected[j][i]=1$都变为$'0'$

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为矩阵$isConnnected$的行数跟列数
- 空间复杂度:
  > $O(n)$，最坏复杂度全部连通，递归栈不超过$n$

### Code
```C++ []
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size(), ans = 0;
        auto dfs = [&](int cur, auto&& dfs)->void {
            for (int i = 0;i < n;++i) {
                if (isConnected[cur][i] == 1) {
                    if (cur == i) {
                        isConnected[cur][i] = 0; continue;
                    }
                    isConnected[cur][i] = 0;
                    isConnected[i][cur] = 0;
                    dfs(i, dfs);
                }
            }
        };
        for (int i = 0;i < n;++i) {
            for (int j = 0;j < n;++j) {
                if (isConnected[i][j] == 1) {
                    ++ans;
                    dfs(i, dfs);
                }
            }
        }
        return ans;
    }
};
```
## 方法二(广度优先搜索)：
### 思路
1. 也可以通过广搜得到省份数量。对于每个城市，如果该城市没被访问过，就通过该城市开始广搜，直到同一个通量的所有城市被访问。

### 复杂度
- 时间复杂度:
  > $O(n^2)$，$n$为矩阵$isConnnected$的行数跟列数
- 空间复杂度:
  > $O(n)$，记录城市访问的数组$visit$为$n$，广搜的队列不会超过$n$

### Code
```C++ []
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size(), ans = 0;
        vector<int> visit(n);
        queue<int> q;
        for (int i = 0;i < n;++i) {
            if (visit[i]) continue;
            q.emplace(i);
            while (!q.empty()) {
                int cur = q.front(); q.pop();
                visit[cur] = 1;
                for (int j = 0;j < n;++j) {
                    if (isConnected[cur][j] == 1 && !visit[j]) {
                        q.emplace(j);
                    }
                }
            }
            ++ans;
        }
        return ans;
    }
};
```

## 方法三(并查集)：
### 思路
1. 使用并查集连通，遍历矩阵，如果为$isConnected[i][j]='1'$，说明$i$与$j$连通
2. 最终连通分量就是省份数量

### 复杂度
- 时间复杂度:
  > $O(n^2*\alpha(mn) )$，$n$为矩阵$isConnnected$的行数跟列数，单次操作并查集时间复杂度为反阿克曼函数$\alpha(mn)$
- 空间复杂度:
  > $O(n)$，并查集$rank，parent$记录$n$个城市数据

### Code
```C++ []
class UnionFind
{
public:
    UnionFind(int n): m_parent(n), m_rank(n) {
        for (int i = 0; i < n; ++i) {
            m_parent[i] = i;
            m_rank[i] = 1;
        }
    }

    void merge(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if (pRoot == qRoot) return;

        if (m_rank[pRoot] < m_rank[qRoot]) {
            m_parent[pRoot] = qRoot;
        }
        else if (m_rank[qRoot] < m_rank[pRoot]) {
            m_parent[qRoot] = pRoot;
        }
        else {
            m_parent[pRoot] = qRoot;
            m_rank[qRoot] += 1;
        }
    }

    int find(int p) {
        if (p != m_parent[p])
            m_parent[p] = find(m_parent[p]);
        return m_parent[p];
    }

    bool is_connect(int p, int q) {
        return find(p) == find(q);
    }

    int diff_union_size() {
        int size = 0;
        for (int i = 0;i < m_parent.size();++i) {
            if (m_parent[i] == i) ++size;
        }
        return size;
    }

private:
    vector<int> m_parent;
    vector<int> m_rank;
};

class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size(), ans = 0;
        UnionFind uf(n);
        for (int i = 0;i < n;++i) {
            for (int j = 0;j < n;++j) {
                if (isConnected[i][j] == 1) uf.merge(i, j);
            }
        }
        return uf.diff_union_size();
    }
};
```