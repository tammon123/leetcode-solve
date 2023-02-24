# 最大堆
## 思路
1. 假设增加一个人后，增率为:$\frac{pass_i+1}{total_i+1}-\frac{pass_i}{total_i}$。因为班级数量是不变的，所以平均通过率与增率正相关，我们每次增加一个人考虑选择增率最大的增加。

2. 因此满足以下条件，$j$班比$i$班更加优先:
   
   $\frac{pass_i+1}{total_i+1}-\frac{pass_i}{total_i}<\frac{pass_j+1}{total_j+1}-\frac{pass_j}{total_j}$

   化简后：$(total_j+1)*total_j*(total_i-pass_i)<(total_i+1)*total_i*(total_j-pass_j)$

3. 我们可以考虑按照上诉规则把班级放入最大堆中，进行$extraStudents$操作。每次弹出栈顶元素，然后把$pass,total$加一再入堆。最后遍历一遍堆，计算出答案。
## 复杂度
- 时间复杂度:
  > $O((m+n)logn)$，$m$为$classes$的大小，$n$等于$extraStudents$，每次堆操作为$O(logn)$
- 空间复杂度:
  > $O(n)$，堆需要存储$classes$的数量

## Code
```C++ []
class Solution {
public:
    struct Ratio {
        int pass, total;
        bool operator<(const Ratio& r) const {
            return (long long)(r.total + 1) * r.total * (total - pass) < (long long)(total + 1) * total * (r.total - r.pass);
        }
    };
        
    double maxAverageRatio(vector<vector<int>>& classes, int extraStudents) {
        priority_queue<Ratio> pq;
        for (auto&& v : classes) {
            pq.push({ v[0], v[1] });
        }
        while (extraStudents) {
            auto [x, y] = pq.top();
            pq.pop();
            pq.push({ x + 1, y + 1 });
            --extraStudents;
        }
        double ans = 0;
        while (!pq.empty()) {
            auto [x, y] = pq.top();
            ans += x * 1.0 / y;
            pq.pop();
        }
        return ans / classes.size();
    }
};
```