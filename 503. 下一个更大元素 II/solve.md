# 单调栈
## 方法一(单调栈)：
### 思路
1. 我们维护一个存储下标的单调栈，遍历到当前元素，如果当前元素小于栈顶元素，就把他入栈，反之，说明栈顶元素找到了比它大的下一个数，弹出栈顶元素，栈顶元素对应的下标的答案为当前元素，循环遍历直到栈顶元素大于等于当前元素
2. 因为数组是循环的，我们可以在遍历一次，之前存栈中的元素就能访问之前元素了
### 复杂度
- 时间复杂度:
  > $O(n)$，n为数组nums的大小
- 空间复杂度:
  > $O(n)$，栈需要维护nums元素，最多为n

### Code
```C++ []
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        stack<int> st;
        int n = nums.size();
        vector<int> ans(n, -1);
        for (int i = 0;i < n;++i) {
            while (!st.empty() && nums[i] > nums[st.top()]) {
                ans[st.top()] = nums[i];
                st.pop();
            }
            st.emplace(i);
        }
        for (int i = 0;i < n;++i) {
            while (!st.empty() && nums[i] > nums[st.top()]) {
                ans[st.top()] = nums[i];
                st.pop();
            }
            st.emplace(i);
        }
        return ans;
    }
};
```

## 方法二(改进方法一，单调栈+数组循环)：
### 思路
1. 可以遍历两倍数据，然后对$n$取模进行数组循环
### Code
```C++ []
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        stack<int> st;

        for (int i = 0;i < (n << 1) - 1;++i)
        {
            while (!st.empty() && nums[i % n] > nums[st.top()])
            {
                ans[st.top()] = nums[i % n];
                st.pop();
            }

            st.emplace(i % n);
        }

        return ans;
    }
};
```