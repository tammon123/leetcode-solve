# 双指针
## 方法一(双指针)：
### 思路
1. 用左右指针分别指向数组的左边跟右边，假设当前左右指针分别指向$x,y$，那么面积为$min(x,y)*d$，假设$x<=y$，如果我们移动$y$，首先距离$d$是减小的，其次，无论$y$是否增大或减小，面积大小都不会再超过当前面积，所以$x$作为面积的左边界丢弃，往右移动。同理$y<=x$一样操作，所以我们只需要每次判断左右指针指向的哪个数字小那边指针移动就可以求出最大面积
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size(), l = 0, r = n - 1;
        int ans = 0;
        while (l < r) {
            ans = max(ans, min(height[l], height[r]) * (r - l));
            if (height[l] <= height[r]) ++l;
            else --r;
        }
        return ans;
    }
};
```