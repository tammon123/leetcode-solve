# 排序+双指针
## 思路
1. 首先我们直接遍历，时间复杂度$O(n^3)$太大了。假设三个数为$a、b、c$，我们只需要排序后使得$a<=b<=c$这样，就不会往前取重复数据。
2. 排序后，为了防止重复元素顺序后重复元素读取，例如：
```
1 2 2 3
```
$(1,2,3)$会读取重复，我们再第一重循环判断与上一个数不同再进行第二重循环。

3. 再第二重循环时候，假如当前$a+b+c=0$，因为后面的$b^\prime>b$，所以需要$c^\prime<c$，那我们只需再第二重循环时候倒叙查找$c$
## 复杂度
- 时间复杂度:
  > $O(n^2)$，n为nums的大小
- 空间复杂度:
  > $O(logn)$，排序空间复杂度

## Code
```C++ []
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> ans;
        sort(nums.begin(), nums.end());
        int n = nums.size();

        for (int i = 0;i < n;++i) {
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int third = n - 1;
            int target = -nums[i];
            for (int second = i + 1;second < n;++second) {
                if (second > i + 1 && nums[second] == nums[second - 1]) continue;
                while (second<third && nums[second] + nums[third]>target) {
                    --third;
                }
                if (second == third) break;
                if (nums[second] + nums[third] == target) {
                    ans.push_back({ nums[i], nums[second], nums[third] });
                }
            }
        }
        return ans;
    }
};
```