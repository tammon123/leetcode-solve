# 递归+回溯/迭代+位运算
## 方法一(递归+回溯)：
### 思路
1. 进行递归操作，一个临时数组表示要这个数是否选择，假如当前元素选择，插入数组，并递归下一个数。递归到当前位置等于$n$，可以把临时数组存入答案中，然后进行回溯，弹出数组最后的元素。
2. 因为数据存在重复元素，我们可以先对数组进行排序，如果上一个元素没被选择，且当前元素等于上一个元素，说明包含当前元素的序列已经被北村如果答案中，我们直接返回不再往下递归。

### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，$n$为数组大小，共有$2^n$种状态，每种状态需要$O(n)$时间，排序需要$O(nlogn)$，渐进原则，最终为$O(n*2^n)$
- 空间复杂度:
  > $O(n)$，临时数组会存储所有元素，栈空间为$O(n)$

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        vector<int> temp;

        auto dfs = [&](bool selectPre, int cur, auto&& dfs)->void {
            if (cur == n) {
                ans.emplace_back(temp);
                return;
            }

            dfs(false, cur + 1, dfs);
            if (cur > 0 && !selectPre && nums[cur] == nums[cur - 1]) return;
            temp.emplace_back(nums[cur]);
            dfs(true, cur + 1, dfs);
            temp.pop_back();
        };
        dfs(false, 0, dfs);
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
3. 因为这题要考虑重复数字，有可能会有重复子集，所以我们可以先排序数组，然后如果迭代到当前进制位数，判断上一位没有选择，且当前位与上一位数字相同，那么就跳过这个状态，继续迭代下一种状态
### 复杂度
- 时间复杂度:
  > $O(n*2^n)$，$n$为数组大小，共有$2^n$种状态，每种状态需要$O(n)$时间，排序需要$O(nlogn)$，渐进原则，最终为$O(n*2^n)$
- 空间复杂度:
  > $O(n)$，临时数组会存储所有元素

### Code
```C++ []
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        vector<int> temp;
        for (int mask = 0;mask < (1 << n);++mask) {
            temp.clear();
            bool flag = true;
            for (int i = 0;i < n;++i) {
                if (mask & (1 << i)) {
                    if (i > 0 && (mask & (1 << (i - 1))) == 0 && nums[i] == nums[i - 1]) {
                        flag = false;
                        break;
                    }
                    temp.emplace_back(nums[i]);
                }
            }
            if (flag) ans.emplace_back(temp);
        }
        return ans;
    }
};
```