# 贪心/双向遍历
## 方法一(贪心)：
### 思路
1. 因为要满足$i < j < k$，使得$nums[i] < nums[j] < nums[k]$，我们只需要顺序遍历，从小到大存储就行，假如先存储两个数据$left,right$，$left$指向第一个元素$nums[0]$，$right$指向$INT\_MAX$，我们从下标$1$遍历数组有：
   - $nums[i]>right$，说明前面有两个数满足条件了，直接返回$true$
   - $nums[i]>left$，说明有两个数满足条件，更新$right$
   - 剩下情况可以直接更新$left$

### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) return false;
        int left = nums[0], right = INT_MAX;
        for (int i = 1;i < n;++i) {
            if (nums[i] > right) return true;
            else if (nums[i] > left) right = nums[i];
            else left = nums[i];
        }
        return false;
    }
};
```
## 方法二(双向遍历):
### 思路
1. 因为只需要找到一组满足条件就行，假设当前数据为$nums[i]$，只需要再$0<=j<=i-1$中找到最小的$nums[j]<nums[i]$，且找到$i+1<=k<=n-1$中找到最大的$nums[i]<=nums[k]$就行。
2. 因此我们可以双向遍历，从左到右找$\min\limits_{0<=j<=i-1}(nums[j])$，从右向左找$\max\limits_{i+1<=k<=n-1}(nums[k])$
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组大小
- 空间复杂度:
  > $O(n)$

### Code
```C++ []
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int n = nums.size();
        if (n <= 2) return false;
        vector<int> left(n), right(n);
        left[0] = nums[0];
        for (int i = 1;i < n;++i) left[i] = min(left[i - 1], nums[i]);
        right[n - 1] = nums[n - 1];
        for (int j = n - 2;j >= 0;--j) right[j] = max(right[j + 1], nums[j]);

        for (int i = 1;i < n - 1;++i) {
            if (left[i] < nums[i] && nums[i] < right[i]) return true;
        }
        return false;
    }
};
```