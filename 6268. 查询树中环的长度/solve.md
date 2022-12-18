# 完全二叉树特性
## 思路
1. 按照题目意思其实是求公共祖先路径之和
2. 根据特性有，当$i=1$时，结点为根结点；当$i>1$时，$parent_i=\lfloor i/2 \rfloor$。我们只需要循环遍历结点$a_i、b_i$找到他们公共祖先结点的路径和加上环$(+1)$，就是结果
3. 当$a_i \neq b_i$时：
- 如果$a_i > b_i$，$a_i/=2$
- 如果$b_i>a_i$，$b_i/=2$
## 复杂度
- 时间复杂度:
  > $O(mn)$，m为queries的大小，n为深度
- 空间复杂度:
  > $O(1)$

## Code
```C++ []
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>>& queries) {
        vector<int> ans;
        for (auto&& q : queries) {
            int edge = 0, a = q[0], b = q[1];
            while (a != b) {
                if (a > b) a /= 2;
                else b /= 2;
                ++edge;
            }
            ans.emplace_back(edge + 1);
        }
        return ans;
    }
};
```