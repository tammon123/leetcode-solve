# 排序
## 方法一（排序）：
### 思路
1. 想要移动次数最少，那么同学应该移动到相应距离最近的位置最合适，即将座位与同学距离都排序后，按照下标算出距离差，最终和就是最小距离。
### 复杂度
- 时间复杂度:
  > $O(nlogn)$，nlogn为排序复杂度，n为座位的数量，遍历数组为O(n)，总体复杂度为O(nlogn)
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int minMovesToSeat(vector<int>& seats, vector<int>& students) {
        sort(seats.begin(), seats.end());
        sort(students.begin(), students.end());
        int ans = 0;
        for (int i = 0;i < seats.size();++i) {
            ans += abs(students[i] - seats[i]);
        }
        return ans;
    }
};
```
