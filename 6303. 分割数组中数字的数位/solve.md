# 模拟
## 方法一(模拟)：
### 思路
1. 直接模拟，注意拆分数字的时候从后往前拆分，需要反转结果再插入
### 复杂度
- 时间复杂度:
  > $O(nlogk)$，$n$为数组$nums$的大小，每个数字需要$O(logk)$时间拆分
- 空间复杂度:
  > $O(1)$

### Code
```C++ []
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        vector<int> ans;
        auto get = [&](int x) {
            vector<int> t;
            while (x) {
                t.emplace_back(x % 10);
                x /= 10;
            }
            reverse(t.begin(), t.end());
            ans.insert(ans.end(), t.begin(), t.end());
        };
        for (auto&& num : nums) {
            get(num);
        }
        return ans;
    }
};
```