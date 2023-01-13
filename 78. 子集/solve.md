# 递归+回溯/迭代+位运算
## 方法一(递归+回溯)：
### 思路
1. 我们可以通过递归把所有组合找出来，假如递归到当前元素，我们只需要考虑当前元素及后面元素的选择或不选择。我们用临时数组，插入当前元素表示当前元素选择，并把临时数组存入答案中，表示选择了这个元素后，有了新组合，然后往下一位递归。采用回溯的方法，弹出临时数组最后一位，表示当前元素不选择。

### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，$n$为数组大小，共有$2^n$种状态，每种状态需要$O(n)$时间
- 空间复杂度:
  > $O(n)$，临时数组会存储所有元素，栈空间为$O(n)$

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> ans;
        ans.push_back({});
        vector<int> temp;
        auto dfs = [&](int cur, auto&& dfs)->void {
            for (int i = cur;i < n;++i) {
                temp.emplace_back(nums[i]);
                ans.emplace_back(temp);
                dfs(i + 1, dfs);
                temp.pop_back();
            }
        };
        dfs(0, dfs);
        return ans;
    }
};
```
## 方法二(迭代+位运算)：
### 思路
1. 因为数组数据比较小，我们可以用二进制每一位来表示元素选择不选择，假设二进制当前为选择为$1$，不选择为$0$，那么总共有$2^n$种状态，例如：
```
数组: [1,2,3]
0: 表示都不选
1: 表示选择1
2: 二进制10表示选择2
3: 二进制11表示选择1,2
4: 二进制100表示选择3
5: 二进制101表示选择3,1
6：二进制110表示选择3,2
7: 二进制111表示选择3,2,1
```
2. 我们可以用临时数组来存储数据，每次遍历一个$mask$时，清空数组
### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，$n$为数组大小，共有$2^n$种状态，每种状态需要$O(n)$时间
- 空间复杂度:
  > $O(n)$，临时数组会存储所有元素

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> ans;
        vector<int> temp;
        for (int mask = 0;mask < (1 << n);++mask) {
            temp.clear();
            for (int i = 0;i < n;++i) {
                if (mask & (1 << i)) {
                    temp.emplace_back(nums[i]);
                }
            }
            ans.emplace_back(temp);
        }
        return ans;
    }
};
```