# 排序
## 方法一(排序)：
### 思路
1. 直接排序模拟，拿出子数组$nums[l[i]], nums[l[i]+1], ... , nums[r[i]]$可以重新排列形成等差数列，$answer[i]$的值就是 $true$；否则$answer[i]$的值就是$false$。
### 复杂度
- 时间复杂度:
  > $O(mnlogn)$，m为数组l的长度，nlogn为nums[l[i],r[i]]范围长度排序，最大为nlogn
- 空间复杂度:
  > $O(1)$，answer数组不计入空间复杂度中

### Code
```C++ []
class Solution {
public:
    vector<bool> checkArithmeticSubarrays(vector<int>& nums, vector<int>& l, vector<int>& r) {
        int n = nums.size(), m = l.size();
        vector<bool> ans(m, false);
        for (int i = 0;i < m;++i) {
            vector<int> v(nums.begin() + l[i], nums.begin() + r[i] + 1);
            sort(v.begin(), v.end());
            int d = -1;
            for (int j = 1;j < v.size();++j) {
                j == 1 ? d = v[j] - v[j - 1] : d;
                if (v[j] - v[j - 1] == d) continue;
                d = INT_MAX;
                break;
            }
            if (d != INT_MAX) ans[i] = true;
        }
        return ans;
    }
};
```