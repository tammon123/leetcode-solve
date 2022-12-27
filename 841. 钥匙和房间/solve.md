# 深度优先搜索/广度优先搜索
## 方法一（深度优先搜索）：
### 思路
1. 从$0$房间开始深搜，用数组$visit$标记，防止重复访问，可用一个变量统计所有访问如果最后等于$n$表示全部访问
### 复杂度
- 时间复杂度:
  > $O(m+n)$，n为房间数，m为钥匙总数
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size(), num = 0;
        vector<int> visit(n);
        auto dfs = [&](int cur, auto&& dfs)->void {
            if (visit[cur]) return;
            visit[cur] = 1;
            num++;
            for (auto&& key : rooms[cur]) dfs(key, dfs);
        };
        dfs(0, dfs);
        return num == n;
    }
};
```

## 方法二（广度优先搜索）：
### 思路
1. 也可用广度优先搜索
### 复杂度
- 时间复杂度:
  > $O(m+n)$，n为房间数，m为钥匙总数
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int n = rooms.size(), num = 1;
        queue<int> q;
        q.emplace(0);
        vector<int> visit(n);
        visit[0] = 1;
        while (!q.empty()) {
            int node = q.front(); q.pop();
            for (auto&& key : rooms[node]) {
                if (visit[key]) continue;
                visit[key] = 1;
                q.emplace(key);
                ++num;
            }

        }
        return num == n;
    }
};
```
