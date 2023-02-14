# 思维题
## 方法一(思维题)：
### 思路
1. 首先我们需要知道字母$[a...z]$减去$'a'$后，可表示为$[0...25]$，可巧妙的表示矩阵的坐标点$(i/5,i\%5)$

2. 那么上一个点$(last_i,last_j)$到当前点$(i,j)$的距离，我们可以先上下走$abs(last_i-i)$，然后左右走$abs(last_j-j)$，但是需要注意字母$z$的位置，如果是从上到下走，我们可以先到$4$，然后左右完毕后，再往下走一格。

3. 如果上一个点与当前点重复，无需操作直接插入$'!'$

### 复杂度
- 时间复杂度:
  > $O(n)$，$n$为字符串$target$的长度，遍历$target$需要$O(n)$，考虑上下走不会超过$5$，忽略不计，系统$abs$也忽略
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    string alphabetBoardPath(string target) {
        string ans;
        int begin_x = 0, begin_y = 0;
        for (auto&& c : target) {
            int x = (c - 'a') / 5, y = (c - 'a') % 5;
            if (begin_x == x && begin_y == y) {
                ans.push_back('!');
                continue;
            }
            int diff = abs((x == 5 ? 4 : x) - begin_x);
            char up_down = x >= begin_x ? 'D' : 'U';
            for (int i = 0;i < diff;++i) {
                ans.push_back(up_down);
            }

            diff = abs(y - begin_y);
            char left_right = y >= begin_y ? 'R' : 'L';
            for (int i = 0;i < diff;++i) {
                ans.push_back(left_right);
            }

            if (x == 5) ans.push_back('D');
            ans.push_back('!');
            begin_x = x;
            begin_y = y;
        }
        return ans;
    }
};
```
## 思维优化
### 思路
1. 我们只需要特别考虑$z$这一点，其他点到$z$点，优先向左。$z$到其他点，优先向上。

### Code
```C++ []
class Solution {
public:
    string alphabetBoardPath(string target) {
        string ans;
        int begin_x = 0, begin_y = 0;
        for (auto&& c : target) {
            int x = (c - 'a') / 5, y = (c - 'a') % 5;
            if (begin_x == x && begin_y == y) {
                ans.push_back('!');
                continue;
            }
            int diff = abs((x == 5 ? 4 : x) - begin_x);
            char up_down = x >= begin_x ? 'D' : 'U';
            for (int i = 0;i < diff;++i) {
                ans.push_back(up_down);
            }

            diff = abs(y - begin_y);
            char left_right = y >= begin_y ? 'R' : 'L';
            for (int i = 0;i < diff;++i) {
                ans.push_back(left_right);
            }

            if (x == 5) ans.push_back('D');
            ans.push_back('!');
            begin_x = x;
            begin_y = y;
        }
        return ans;
    }
};
```