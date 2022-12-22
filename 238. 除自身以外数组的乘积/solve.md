# 前缀积后缀积
## 方法一(前缀积后缀积)：
### 思路
1. 可以用前缀和的思想，我们要求当前$i$位置的结果，可以用$i$之前的积乘以$i$之后的积，即前缀积后缀积

### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组的大小
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n), right(n), ans(n);
        left[0] = 1, right[n - 1] = 1;
        for (int i = 1;i < n;++i) {
            left[i] = nums[i - 1] * left[i - 1];
        }
        for (int j = n - 2;j >= 0;--j) {
            right[j] = nums[j + 1] * right[j + 1];
        }
        for (int i = 0;i < n;++i) ans[i] = left[i] * right[i];
        return ans;
    }
};
```
## 方法二(空间复杂度O(1)):
### 思路
1. 因为答案数组不视为空间复杂度相关，那么可以利用这个空间，先求出前缀积，然后用一个变量$r=1$，进行后缀积的运算
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组的大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n);
        ans[0] = 1;
        for (int i = 1;i < n;++i) ans[i] = nums[i - 1] * ans[i - 1];
        int r = 1;
        for (int j = n - 1;j >= 0;--j) ans[j] = r * ans[j], r *= nums[j];
        return ans;
    }
};
```